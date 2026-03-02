---
tags:
  - EN
  - Unfinished
---

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Introduction

To paraphrase the documentation : 

```
The Sleuth Kit® (TSK) is a library and collection of command line tools that allow you to investigate disk images. The core functionality of TSK allows you to analyze volume and file system data. The library can be incorporated into larger digital forensics tools and the command line tools can be directly used to find evidence.
```

```
Analyzes raw (i.e. dd), [Expert Witness](https://github.com/libyal/libewf-legacy) (i.e. E01/EnCase), [VHD](https://github.com/libyal/libvhdi), [VMDK](https://github.com/libyal/libvmdk), and AFF file system and disk images.

TSK support the following filesystems : NTFS, FAT, ExFAT, APFS, UFS 1, UFS 2, EXT2FS, EXT3FS, Ext4, HFS, ISO 9660, and YAFFS2
```

# Problem with XFS 

XFS was previously not supported and this fork was previously used : https://github.com/isciurus/sleuthkit

But seems to have been merged to the main (develop) branch (https://github.com/sleuthkit/sleuthkit/pull/1461), so use TSK from the main rep directly.

# Commands cheatsheet

- Create bodyfile : `fls -r -m / image.dd > body.txt`
- Create timeline from bodyfile with user + groups : `mactime -b body.txt -z UTC <startingDate -p -q > timeline.txt



<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://www.sleuthkit.org/sleuthkit/
- https://www.sleuthkit.org/sleuthkit/desc.php

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-26 14:31 | Page creation |
