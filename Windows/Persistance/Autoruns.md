---
tags:
  - Persistance
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
## Introduction

Windows defines a list of keys an folders that allow to execute files at start. Those keys and folders are often abused by attackers and constitute a good starting point for the research of persistence.

<u>General method : </u>

Use the tool Autorun from the Sysinternal suite : 
https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns

<b>Startup folders</b>

- ``%SystemDrive%\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
- `%userprofile%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup


<u>Exemple de Registry Run/RunOnce Keys :</u>

Some are only available for Win7 and others for Win10, for further references check the sources.
```
 HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServices
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
- HKEY_CURRENT_USER\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
- HKEY_CURRENT_USER\Software\Microsoft\Windows NT\CurrentVersion\Windows
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServicesOnce
- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell Folders
- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\User Shell Folders
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Userinit
- KEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnceEx
- HKEY_LOCAL_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
- HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager
```

<u>A word on registers and transaction logs : </u>

Since Win2k the OS uses transaction logs, which are -to be simple- hives containing the operations in progress and that could not be written as the hive was locked or corrupted.
If the analyst isn't able to gather the transactions logs, the hive is considered "dirty".

<u>Localisation des transaction logs</u>

Classical path is : 
- ``%systemdrive%\windows\system32\config\\

and in the form of ``<Hive_Name>.LOG\d

<u>Exemple of artifacts to analyze in the hives :</u>

At the start, Registry Explorer indicates the keys that can be interesting to check, but it is interesting to check the following keys : 

https://github.com/carlospolop/hacktricks/blob/master/forensics/basic-forensic-methodology/windows-forensics/interesting-windows-registry-keys.md

### TOOLS

Registry explorer from Zimmerman Documentation :
https://aboutdfir.com/toolsandartifacts/windows/registry-explorer-recmd/

Download : https://ericzimmerman.github.io/#!index.md




<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- Autorun : https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns
- Information on autostart : https://pentestlab.blog/2019/10/01/persistence-registry-run-keys/
- Information on autostart 2 : https://www.fuzzysecurity.com/tutorials/19.html 
- Information on autostart 3 : https://attack.mitre.org/techniques/T1547/001/
- "Documentation" of registry explorer : https://aboutdfir.com/toolsandartifacts/windows/registry-explorer-recmd/
- Interesting registry key 1 : https://github.com/carlospolop/hacktricks/blob/master/forensics/basic-forensic-methodology/windows-forensics/interesting-windows-registry-keys.md
Interesting registry keys 2 : https://research.splunk.com/endpoint/f5f6af30-7aa7-4295-bfe9-07fe87c01a4b/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:07 | Page creation |

