---
tags:
  - EN
  - Linux
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

From a forensic point of vue Plesk is interesting as its capabilities allow to completely control a web server without necessarily need to have access to the file system. Its capabilities include : 

- **Web server administration** (Apache / Nginx configuration)
- **Virtual host management** (domains → document roots)
- **File & directory management** (ownership, permission)
- **PHP management** (version, handler, PHP‑FPM, settings)
- **Database management** (MySQL / MariaDB, users, credentials)
- **User access control** (Plesk users, FTP, SSH)
- **Scheduled tasks** (server‑level cron jobs)
- **SSL/TLS management** (certificates, HTTPS config)
- **Backups / snapshots / restores**
- **WordPress management** (install, update core, plugins, themes)

It's important to note that as Plesk configures different services, for example Apache, it let the tools logs in their respective logs location.

The principal source of logs for Plesk in itself is the following : 
- /var/log/plesk/panel.log

It logs the following : 
- Panel connections success/failures
- IP
- Plesk users
- Actions post authentication

Use those connections in coordination with /var/log/auth.log as usual.

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://www.plesk.com/features/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-26 14:43 | Page creation |