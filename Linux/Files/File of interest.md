---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
### General Logs

| Path / File                      | Main Content                      | Forensic Value (What to Quickly Look For)                                |
| -------------------------------- | --------------------------------- | ------------------------------------------------------------------------ |
| `.bash_history`                  | Shell command history             | Attack commands, tool downloads, recon activity, payload execution       |
| `.mysql_history`                 | MySQL command history             | DB dumps, backdoor user creation, sensitive data access                  |
| `.ftp_history`                   | FTP client history                | Data exfiltration, webshell/payload uploads                              |
| `.viminfo`                       | Vim file/command history          | Identify sensitive files edited (passwd, sudoers, webroot files)         |
| `.git/logs`                      | Local commit history              | Malicious code insertion/removal, dev timeline reconstruction            |
| `.gitconfig`                     | Git identity/config               | User attribution, identity correlation                                   |
| `/etc/passwd`                    | System user accounts              | Suspicious accounts, abnormal UID 0 users, unexpected interactive shells |
| `/etc/group`                     | System groups                     | Silent privilege escalation via sudo/docker/adm group additions          |
| `/etc/fstab`                     | Disk mount configuration          | Malicious persistent mounts, overlay rootkits, suspicious NFS mounts     |
| `/etc/ssh/sshd_config`           | SSH server configuration          | Root login enabled, password auth enabled, hidden ports, forced commands |
| `/etc/sudoers`                   | Sudo privilege rules              | NOPASSWD backdoors, dangerous wildcard rules                             |
| `.ssh/authorized_keys`           | Allowed SSH public keys           | Attacker persistence via SSH keys, suspicious `command=` options         |
| `.ssh/known_hosts`               | Known SSH server fingerprints     | Connection history, lateral movement indicators                          |
| `/etc/ld.so.preload`             | Forced preloaded shared libraries | Userland rootkits, libc hooking (**major red flag**)                     |
| `/dev/shm/`                      | Shared memory (RAM-backed)        | Fileless malware staging, temporary payload execution                    |
| `/dev/`                          | System device interfaces          | Fake devices, kernel rootkit artifacts                                   |
| `SUID files`                     | Root-privileged executables       | Privilege escalation vectors, backdoored binaries                        |
| `SGID files`                     | Group-privileged executables      | Lateral privilege abuse, group data access abuse                         |
| `/etc/cron* /var/cron* /etc/at*` | Scheduled task systems            | Persistence jobs, periodic reverse shells, payload downloaders           |
| `/etc/pam.d`                     | PAM authentication modules        | Authentication backdoors, credential harvesting, MFA bypass              |
| `/var/www/`                      | Web server root directory         | Webshells, droppers, crypto miners, abused upload directories            |
### Resources

- Personal knowledge + (shamefully) : IA for file/PATH descriptions


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-05 16:28 | Page creation |

