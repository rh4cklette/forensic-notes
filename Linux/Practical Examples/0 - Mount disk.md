---
tags:
  - EN
  - Unfinished
---

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

Before mounting a disk image, there are multiple things to first understand : 
- What's the format of the disk image sent to us ?
- What is the filesystem ?
- Is it sitting on top of LVM ?
- Does our toolkit allows to analyse this kind of filesystem ?

# Lexicon

A bit of vocabulary is needed before understanding the rest of this page.

<u><b>Block device</b></u> : `Block special files or block devices provide buffered access to hardware devices, and provide some abstraction from their specifics. Unlike character devices, block devices will always allow the programmer to read or write a block of any size (including single characters/bytes) and any alignment. Most systems create both block and character devices to represent hardware like hard disks. FreeBSD and Linux notably do not; the former has removed support for block devices, while the latter creates only block devices` 

<b><u>Nbd</u></b> : Network Block Device. To paraphrase Wikipedia : 
`On Linux, network block device (NBD) is a network protocol that can be used to forward a block device (typically a hard disk or partition) from one machine to a second machine. As an example, a local machine can access a hard disk drive that is attached to another computer.`

<b><u>Filesystem</b></u> : In this page, it designs file organization in a physical or logical volume, that can be of different type, ex NTFS, FAT, ext4, ZFS etc. 

<b><u>Raw device</u></b>  : Raw device is a special kind of logical device associated with a character device file that allows a storage device such as a hard disk drive to be accessed directly, bypassing the operating system's caches and buffers (although the hardware caches might still be used). Applications like a database management system can use raw devices directly, enabling them to manage how data is cached, rather than deferring this task to the operating system.

# Convert different image type

## VMDK

When receiving multiple VMDK images, it's important to check if it's a full image or an incremental image. This can be seen with the size of the images, or with the following :

`head <image.vmdk> | grep -a "parentFileNameHint" `

![[Pasted image 20260127105429.png]]

This gives us multiple information :
- Vmware image type : monolithicSparse
- Parent file name : The base image this incremental needs.

If launched on the base image : ![[Pasted image 20260127110305.png]]

If two branches have the same parent, it means they are parallel branches : 
![[Pasted image 20260127111705.png]]

![[Pasted image 20260127111748.png]]

Now we can use `qemu-img` to create a raw image, containing the incremental we want + the base disk.

```
% apt-file search qemu-img
qemu-utils: /usr/bin/qemu-img
```

Install qemu-img, then : 

`qemu-img convert -p -O raw <snapshot.vmdk> <VM_name.raw>

It's important to note that `qemu-img` uses the `parentFileNameHint` to reconstruct the raw disk, so make sure to have all the needed files in the same folder.
## OVA

An OVA is basicaly a zip containing a VMDK, and other files needed for the hypervisor.
Unzip the OVA then refer to the [[0 - Mount disk#VMDK]] section.

## VHD

The Virtual Hard Disk (VHD) commonly uses the .vhd extension.

This format is used to store virtual disk images by:

- Microsoft Virtual PC
- Microsoft Virtual Server
- Microsoft Hyper-V Server

For this we usually use Arsenal Image Mounter to mount the image.

For Linux, use :
- `modprobe nbd max_part=16`
- `qemu-nbd --connect=/dev/nbd0 --read-only disk.vhd`
- `kpartx -av --readonly /dev/nbd0`

Then mount the block in function of the filesystem, with the options described bellow.

Or convert in raw with : 
- `qemu-img convert -p -f vhd -O raw disk.vhd disk.raw`

## VDI

Used by VirtualBox, either use qemu-img to convert it to a raw disk image :
```
qemu-img convert -O raw monDisque.vdi monDisque.raw
sudo mount -o loop,ro monDisque.raw /mnt/vdi
```

Or directly use `qemu-utils` and `nbd` :

```
sudo modprobe nbd max_part=8
sudo qemu-nbd -c /dev/nbd0 monDisque.vdi
sudo fdisk -l /dev/nbd0
```

Mount needed partition(s):

```
sudo mount -o ro,noload,noatime /dev/nbd0p1 /mnt/vdi
```

And when finished : 

```
sudo umount /mnt/vdi
sudo qemu-nbd -d /dev/nbd0
```

# Mounting

Before mounting a disk image, <b><u>ALWAYS</u></b> create a copy of the disk image to work on it. Then hash both images with SHA256n confirm that both image have the same hash.

The first step is to identify what's in the disk image you received. 
The following commands are usefull : 
- `parted <image_name> print
- `blkid <image_name>`
- `fdisk -l <image_name>` 

Fdisk is more usefull as it will print sectors size, which are used for loop creation.
For exemple :
- `fdisk -l test_qemu_img.raw`

```bash
Disk test_qemu_img.raw: 60 GiB, 64424509440 bytes, 125829120 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x8a4ac9c8

Device             Boot   Start       End   Sectors  Size Id Type
test_qemu_img.raw1 *       2048   1050623   1048576  512M  b W95 FAT32
test_qemu_img.raw2      1052670 125827071 124774402 59.5G  5 Extended
test_qemu_img.raw5      1052672 125827071 124774400 59.5G 83 Linux
```

<b><u>Method 1 : Loop</u></b>

We want to mount Linux partition in a loop, so we'll do : 
- `losetup --find --read-only --offset <Start*Sector_Size> disk.img`
Which is : 
- `losetup --find --read-only --offset <1052672*512> disk.img

If you imaged only a partition, you wont have to do this method and can directly mount the partition through losetup + mount without directly using offset.

For the mount option on your loops, refer directly to the correct filesystem bellow.

<b><u>Method 2 : kpartx</u></b>

Mount the raw disk in a loop : 
- `losetup --find --read-only --show disk_xfs.img`
Then on your loop : 
- `kpartx -av --readonly /dev/loop<X>`

This should create multiple blocks in : `/dev/mapper/loop<X>p<Y>`
Then identify the filesystems in your blocks with `blkid`

For the mount option on your blocks, refer directly to the correct filesystem bellow.

For unmounting :
- `umount /mnt/<yourmountPoint>`
- `kpartx -dv /dev/loop<X>`
- `losetup -d /dev/loop<X>`

## EXT3/EXT4

For EXT3 / EXT4 and to preserve forensic evidences, it is important to mount the image as <b>read-only</b> and specify <b>noload</b>. Why ? Because Ext3 or Ext4 will replay its journal if the filesystem is dirty

First identify what's in the disk image you received, and prepare the loop with your prefered method. Refer to [[#Mounting]] if needed.

And mount it : `
- `mount -o ro,noload,noatime /dev/loop<YourLoopNumber> /mnt/forensic`

Or the all in one command:
- `mount -o ro,noload,noatime,loop,offset=<yourOffset> disk.img /mnt/forensic`

## ZFS

ZFS is funcdamentaly differnt from EXT4.
When EXT4 usually goes like : 
- ext4 + LVM + mdadm

ZFS does this :
- Disks → vdevs → zpool → datasets

<b>Vdevs</b> are : mirror / RaidzX or a simple disk
<b>Zpool</b> is a global memory space, seen like a virtual disk
<b>Datasets</b> are filesystems. Each dtaset has his own compression, mountpoint, snapshots etc

Remember hat ZFS is not a "passive" filesystem and not mounting without read-only will alter the evidences.

To mount a ZFS dataset, it's relatively simple : 

Create a read-only loop : 
- `losetup --find --read-only --show disk_zfs.raw`

Tell ZFS to use the loop :
- `zpool import`

This should yield the name of the datapool.
Import it in zpool : 
- `zpool import -o readonly=on -o cachefile=none -N <datapool>`

List the datasets : 
- `zfs list`

And then mount it as read only, even if we already set up readonly twice : 
- `zfs mount -a -o ro`

By default it is mounted in : 
- `/mnt/datapool/`

## BTRFS

Btrfs is a **copy-on-write (CoW)** filesystem designed for Linux, with built-in snapshots, checksums, subvolumes, and RAID-like features.  
All data and metadata are checksummed, and corruption can be detected (and sometimes repaired).  
Because it is CoW and actively manages metadata, **mounting it incorrectly can modify the filesystem**, even in read-only scenarios.

First identify what's in the disk image you received, and prepare the loop with your prefered method. Refer to [[#Mounting]] if needed.

Then :
- `mount -t btrfs -o ro,nologreplay,norecovery /dev/mapper/loopXp<X> /mnt/<yourMountPoint>`

## XFS

From Wikipedia : `XFS excels in the execution of parallel [input/output](https://en.wikipedia.org/wiki/Input/output "Input/output") (I/O) operations due to its design, which is based on [allocation groups](https://en.wikipedia.org/wiki/Allocation_group "Allocation group") (a type of subdivision of the physical volumes in which XFS is used – also shortened to _AGs_). Because of this, XFS enables extreme scalability of I/O threads, file system bandwidth, and size of files and of the file system itself when spanning multiple physical storage devices.`

First identify what's in the disk image you received, and prepare the loop with your prefered method. Refer to [[#Mounting]] if needed.

Then : 
- `mount -t xfs -o ro,norecovery /dev/loop<yourloop> /mnt/<yourMountpoint>`

## UFS

UFS is a **traditional Unix filesystem**, historically used by BSD systems (FreeBSD, NetBSD, OpenBSD).  
It exists in multiple variants (UFS1, UFS2) and may use **soft updates** or journaling.

First identify what's in the disk image you received, and prepare the loop with your prefered method. Refer to [[#Mounting]] if needed.

Then : 
- `mount -t ufs -o ro,ufstype=ufs2 /dev/mapper/loop<X>p<Y> /mnt/<yourMountPoint>`

## LVM 

First it's important to understand how LVM works : 
![[Pasted image 20260128153816.png]]
Usually when using LVM the following vocabulary is used : LV (Logical Volumes), VG (Volume Group), PV (Physical volumes)

PVs can be harddisk, harddisk partitions, RAID volumes, or LUNs.

VGs concatenate thoses PVs into volume groups, it's like pseudo hard drives.

LV : Those VGs are then segmented in volume groups, formated and then mounted in filesystems or used as raw devices. LVs are like pseudo partitions.

Usefull commands to know are : 
- pvs - reports information about LVM physical volumes
- pvscan - scans all supported LVM block devices
- lvdisplay - logical volume in-depth information
### Method 1 : NBDs

This method is clearly copied from https://ponderthebits.com/2017/03/quicker-mounting-and-dismounting-of-lvms-on-forensic-images/.

`ls -l /dev/nbd*`

**Ensure you have an available NBD**  
`# ls -l /dev/nbd*`

**If no device files (or not enough) exist as /dev/nbd*, create as many as needed**  
`# for i in {0..<z>}; do mknod /dev/nbd$i b 43 $i; done`  
*Where `<z>` is the number of devices you need, minus one

**Mount the image file**  
`# qemu-nbd -r -c /dev/nbd<x> image.<extension>`  
* `-r`: read-only  
* `-c`: connect image file to NBD device  
*Where `<x>` is the appropriate block device number (typically starting at 0) and `<extension>` is a supported [QEMU Image Type](https://en.wikibooks.org/wiki/QEMU/Images#Image_types) (raw, cloop, cow, qcow, qcow2, vmdk, vdi, vhdx, vpc)

Note that this will need to be done for each image that is a part of the LV group. For example, if there are 3 different VMDK files that together comprise one or more LV groups, you would do the following (ensuring the associated /dev/nbd devices have already been created before issuing the below commands):

`# qemu-nbd -r -c /dev/nbd0 image_0.vmdk`  
`# qemu-nbd -r -c /dev/nbd1 image_1.vmdk`  
`# qemu-nbd -r -c /dev/nbd2 image_2.vmdk`

_Keep in mind that the order of the below commands is critical to successful mounting of LVM’s._

**Load the LVM module if not already loaded**  
`# modprobe dm_mod`

**Ensure you have enough available loopback devices (one for each nbd device)**  
`# ls -l /dev/loop*`

**I****f not enough loopback devices exist, create as many as needed**  
`# for i in {0..<z>}; do mknod /dev/loop$i b 7 $i; done`  
*Where `<z>` is the number of devices you need, minus one

**Set up a loopback device for each image that is part of the LV group**  
`# losetup -r [-o <byte_offset>] -f [/dev/loop<x>] /dev/nbd<x>`

**Read partition tables from each loopback device to create mappings**  
`# kpartx -a -v /dev/loop<x>`

**List the available Physical Volume Groups (VG’s)**  
`# pvdisplay`

**List the available Logical Volume Groups (VG’s)**  
`# lvdisplay`

**(Optional) If not listed, re-scan the mounted volumes to identify the associated VG’s**  
`# pvscan`  
`# lvscan`  
`# vgscan`

**Activate the appropriate VG’s**  
`# vgchange -a y <VG>`  

The recombined LVM volume(s) will now be available at _`/dev/mapper/<VolumeGroup>-<VolumeName>

**Mount the LVM Volume(s)**  
`# mount [options] /dev/mapper/<VolumeGroup>-<VolumeName> /mnt/point`

Congratulations. You (should) now have filesystem access to the given LVM(s)!

Interested in why we use the given numbers of “`43`” and “`7`” for the `mknod` command?

The mknod command is structured like the following: `mknod <device> <type> <major_#> <minor_#>`

#### Unmount

ismount each mounted filesystem : `# umount /mnt/point`
De-activate each activated Volume Group : `# vgchange -a n <VG>`
Remove partition mappings for each loop device : `# kpartx -d -v /dev/loop<x>`
Remove each loopback device : `# losetup -d /dev/loop<x>`

**(Optional) Force remove an LVM**  
`# dmsetup remove -f <VG>`

Keep in mind the `dmsetup` command above is considered deprecated and not suggested for use. However, I provide it as I have had to use it at times in the past when a VG simply would not detach using the appropriate commands. That said, if all else fails, reboot

### Method 2 : Do not cite the deep magic ...

This method is clearly copied from https://4n6tacohut.blogspot.com/2016/09/mounting-and-imaging-logical-volume.html.

Install LVM : `sudo pat-get install lvm2`

Mount each raw image to a loop device : `losetup /dev/loopX --read-only disk.img`

List PVs to know if it succeeded : `sudo pvs`

Show your LVs : `sudo lvdisplay`

Get the LV path : `sudo fdisk -l`

Mount the LV path (should be something like /dev/mapper/VolGroupX) to the path you want : `sudo mount -o ro,noload /dev/mapper/VolGroupX /mnt/<yourMountPoint>`

Ls inside your mountpoint to confirm it works.

If needed one can dd the VolumeGroup /dev/mapper/VolGroupX to create a valid image file.

#### Unmount

Unmounting the mount point : `sudo umount /mnt/<yourMountPoint>`
Get the VolumeGroup Name  : `sudo dmsetup info`
Remove the VolumeGroup : `sudo dmsetup remove VolGroupX`

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Resources

The sources used for the creation of this article are the following :
- Identify what's in the image : https://unix.stackexchange.com/questions/438292/check-disk-image-file-system-type
- https://docs.openstack.org/image-guide/modify-images.html
- Man mount : http://linux.die.net/man/8/mount
- Forensics wiki : https://forensics.wiki/virtual_hard_disk_%28vhd%29/
- Network block device : https://en.wikipedia.org/wiki/Network_block_device
- Device file / block device : https://en.wikipedia.org/wiki/Device_file
- https://fr.wikipedia.org/wiki/Ext4
- Mounting LVM on forensic images : https://ponderthebits.com/2017/03/quicker-mounting-and-dismounting-of-lvms-on-forensic-images/
- Mounting LVM with deep magic : https://4n6tacohut.blogspot.com/2016/09/mounting-and-imaging-logical-volume.html
- LVM Scheme : https://wiki.zikossworld.com/doku.php?id=informatique:linux:lvm
- LVM : https://fr.wikipedia.org/wiki/Gestion_par_volumes_logiques

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-27 09:53 | Page creation |


