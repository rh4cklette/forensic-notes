---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

This page is clearly inspired from the excellent articles from 0xMatheuZ https://matheuzsecurity.github.io/hacking/linux-threat-hunting-persistence/ and https://pberba.github.io/security/2021/11/22/linux-threat-hunting-for-persistence-sysmon-auditd-webshell/. The rest is from personal experience or sources written in Resources section.

# Hunt Malicious SSH Keys 

![[Pasted image 20260202151014.png]]

Exemple of commands :
- Search ~/.ssh/ for each user : `for home_dir in /home/*; do [ -d "$home_dir/.ssh" ] && echo "HOME \"$(basename "$home_dir")\""; [ -d "$home_dir/.ssh" ] && cat "$home_dir"/.ssh/*; done` 
- List ssh folder for users : `ls -la -R /home/*/.ssh`
# Cron persistance 

![[Pasted image 20260202150810.png|300x300]]

Exemple of commands : 
- List all anacron jobs : `for f in /etc/cron*/*; do [ -f "$f" ] && echo "===== $f =====" && cat "$f" && echo; done`
- `cat /etc/crontab`
- `ls -la /var/spool/*`
- `ls -la -R /var/cron*`

# Bashrc

![[Pasted image 20260202151848.png]]

<b><u>Exemple of commands :</u></b> 
- `cat /home/*/{.bashrc,.zshrc}` 
- `ls -la /home/*/{.bashrc,.zshrc}`

# APT 

Check apt.conf.d in `/etc/apt/apt.conf.d` for malicious apt conf used when doing `apt update`

<b><u>Exemple of commands :</u></b> 
- `ls -la -R /etc/apt/apt.conf.d`

# Privileged user & SUID bash

![[Pasted image 20260202153710.png]]

Attacker can create new users for persistance, putting them in sudoers to give them root access, add SSH key (refer to [[Persistence general checklist#Hunt Malicious SSH Keys]]), exploit / use SUID binaries.

<b><u>Exemple of commands :</u></b> 

- Hunt for setuid <b>or </b>setgid binaries : `find / -perm -4000 -o -perm -2000 -exec ls -ldb {} \;`
- `ls -la /etc/sudoers.d` 
- `cat /etc/sudoers` 
- `cat /etc/groups`
- `cat /etc/passwd`


# Systemd services

![[Pasted image 20260203142052.png]]

Exemple of commands : 

<b><u>For systemd services executing at system-level :</u></b> 
- `ls -la /etc/systemd/system`
- `ls -la /lib/systemd/system`
- `ls -la /run/systemd/system`
- `ls -la /usr/lib/systemd/system`
- `find / -path "*/systemd/system/*.service" -exec grep -H -E "ExecStart|ExecStop|ExecReload" {} \; 2>/dev/null`

<b><u>For systemd services executing at user-level :</u></b> 
- `ls -la /home/*/.config/systemd/user/`
- `/etc/systemd/user`
- `~/.local/share/systemd/user`  
- `/run/systemd/user`  
- `/usr/lib/systemd/user`
- `find / -path "*/systemd/user/*.service" -exec grep -H -E "ExecStart|ExecStop|ExecReload" {} \; 2>/dev/null`

# PAM Backdoor

![[Pasted image 20260203144402.png]]

`PAM Backdoor is a well-known persistence technique, it works by manipulating the Pluggable Authentication Modules (PAM) authentication system. This allows unauthorized access to the system by granting a specific user privileged access regardless of correct credentials.`

<b><u>Detection :</u></b> 
- Consist more or less to find malicous pam_unix.so dropped to disk, check [[TSK]] on how to create a timeline of modified files

# Init.d persistence / rc.local persistence

![[Pasted image 20260203145915.png]]

## Init.d

```
init.d are the scripts that are executed at machine startup, that is, as soon as the machine is turned on, the scripts that are on it are executed, and this is like gold for an attacker, as he can simply add a reverse shell payload, or execute any script he wants, as soon as the machine starts/restarts.
```

<b><u>Detection :</u></b> 
- `ls -la /etc/init.d/*` 
- `ls -la /etc/rc*.d/`
## rc.local 

```
rc.local is a startup script in Linux used to execute custom commands or scripts during the system boot process, however it has been replaced by more modern methods such as systemd service units.

An attacker could add a reverse shell to /etc/rc.local and every time your machine is started, the content on it will be executed with root privileges, thus providing very good and effective persistence.
```

<b><u>Detection :</u></b>  
- `/etc/rc.local` 
- `/lib/systemd/system/rc-local.service.d` 
- `cat /run/systemd/generator/multi-user.target.wants/rc-local.service`

# Infected client software 

![[Pasted image 20260203153331.png]]

Infected packets are used by attacker to blend in. For detection in this kind of scenario, usually you can use the tool [[UAC]] to collect the hash of packages and use it with 

# Webshells

Dependant of the context and the technology used on the server. However. See [[Wordpress]] for some exemples.

<b><u>Detection :</u></b> 
- Simple fast and dirty command for analysis : `grep -rlE 'fsockopen|pfsockopen|exec|shell|system|eval|rot13|base64|base32|passthru|\$_GET|\$_POST\$_REQUEST|cmd|socket' /var/www/html/*.php | xargs -I {} echo "Suspicious file: {}"`
- The best is always to check for file created in suspicious path through timeline made with [[TSK]] for exemple. 



<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- Threat hunting Linux : https://matheuzsecurity.github.io/hacking/linux-threat-hunting-persistence/
- Setuid/SetGid : https://stackoverflow.com/questions/2189976/find-suid-and-gid-files-under-root
- LinuxThreat hunting : https://pberba.github.io/security/2021/11/22/linux-threat-hunting-for-persistence-sysmon-auditd-webshell/
- Systemd service persistence : https://redcanary.com/blog/threat-detection/attck-t1501-understanding-systemd-service-persistence/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-02 14:48 | Page creation |