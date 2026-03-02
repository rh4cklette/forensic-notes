---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Introduction

The objective of this file is to recap what is needed for a full, in depth forensic analysis. It does not mean you have to check the full list everytime you're doing forensics as it also depends on what intrusion you suspect and how long you have for the analysis. This list is built from experience and some might want to add / remove stuff to it.

# List

### Persistance [[Persistence general checklist]]
- Services
- Cron tasks/scheduled tasks
- Webshells
- Modules (see [[UAC]] for `debsums`)
- SSH Keys

### Network
- Network connections (either through ps -aux or equivalent)
- Access logs if forensicating a Web server

### Users 
- Logins / failures
- Malicious users / groups ( /etc/passwd, /etc/group, /etc/sudoers)
- Terminal history (~/.bash_history)

### Files and folders
- Analyse timeline to understand what was created/modified ([[1 - Create timeline]])
- SUID/GUID files ([[Persistence general checklist]])

### Logs
- See [[Logs files]] and [[UAC]]


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-02 14:21 | Page creation |