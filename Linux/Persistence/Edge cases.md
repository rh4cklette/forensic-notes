---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Rootkits

![[Pasted image 20260203145734.png]]

Heh, you are on your own.
More seriously this should get a dedicated page.

# ACL Persistence



# MOTD Persistence

Check the following for weird MOTD containing reverse shell.

Detection : 

- `ls -la /etc/update-motd.d/*` 
- `/usr/lib/update-notifier/update-motd-updates-available` 
- `cat /etc/motd` 
- `find / -name "*motd*" 2>/dev/null`

# Hidden process mounted

T1564.013

Detection opportunity : 
-  Alert if /proc/[pid] is **mounted to tmpfs or external filesystems**
- check /etc/fstab for /proc/pid mount.

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- Hidden process mounted explained : https://www.group-ib.com/blog/linux-pro-manipulation/
- Hidden process mounted in actual attack : https://www.group-ib.com/blog/unc2891-bank-heist/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-03 15:02 | Page creation |