---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Mount SMB Share

- `sudo mount -t cifs -o username=<user>,password=<passord> //<shareip>/<sharename> <mount_point>`

Exemple : 

`sudo mount -t cifs -o username=REDACTED,password=REDACTED //192.168.25.9/sambashare sambashare/`
# Journalctl

Target specific file(s) : `journalctl --file user-10004* |less`
Target folder + start date  + end date : `journalctl -D . --since "2025-12-15 00:00:00" --until "2025-12-18 00:00:00`
Target specific unit : `journalctl -D . --since "2025-12-15 00:00:00" -u sshd`

/!\ On specific distros (Ubuntu/Debian for example) some units do not have the expected name. For example, on Ubuntu ssh unit is named : ssh.service, so "-u sshd" will not yield result. /!\

Find accepted SSH connections : `journalctl -D . --since "2025-12-15 00:00:00" -u ssh | grep -v "Invalid user" | grep -v "Connection closed by" | grep ": Accepted"`

There are tons of other options, see journalctl(7) if needed, but some exemples are : 

- `journalctl _SYSTEMD_UNIT=avahi-daemon.service _PID=28097 + _SYSTEMD_UNIT=dbus.service`
- _``` SYSTEMD_UNIT=_name_.service + UNIT=_name_.service _PID=1 + OBJECT_SYSTEMD_UNIT=_name_.service _UID=0 +COREDUMP_UNIT=_name_.service _UID=0 MESSAGE_ID=fc2e22bc6ee647b6b90729ab34a250b1```

# Analyse WTMP / BTMP

- `last --time-format iso  -f wtmp` 
- `last --time-format iso -f btmp`

# Miscelaneous commands

## DD through network

- Through SSH`dd if=/dev/md0 | gzip -1 - | ssh user@remotehost dd of=/path/folder/md0.gz`
- Through SSH 2 : `$ ssh root@remoteIP "dd if=/dev/vda" | dd of=filename.dd`
- Through SSH 3 `$ ssh root@remoteIP "dd if=/dev/vda | gzip -1 -" | dd of=filename.gz`
- dd to ip that is listening with netcat : `dd if=<partition> bs=1M > /dev/tcp/<ip>/port`.
	ex : dd if=/dev/loop6 bs=1M > /dev/tcp/192.168.0.3/1337

## Load Kernel modules

- `insmod <driver_path>`


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- Man journalctl : https://man7.org/linux/man-pages/man1/journalctl.1.html
- Man journalctl 7 : https://man7.org/linux/man-pages/man7/systemd.journal-fields.7.html
- AskUbuntu ssh service : https://askubuntu.com/questions/1523856/failed-to-restart-sshd-service-unit-sshd-service-not-found
- DD through network : https://www.iblue.team/incident-response-1/mounting-ufs-vmdk-from-netscaler-citrix-adc

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-02 11:55 | Page creation |



