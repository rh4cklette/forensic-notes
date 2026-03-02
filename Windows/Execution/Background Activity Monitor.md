---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Introduction
 
BAM is a Windows service that Controls activity of background applications.
This service exists in Windows 10 only after Fall Creators update - version 1709.
It provides full path of the executable file that was run on the system and last execution date/time. BAM entries older than 7 days are removed during the boot. BAM entries aren’t created for executables on removable media and/or on network shares

### Localisation

- ``HKLM\SYSTEM\CurrentControlSet\Services\bam\UserSettings\<User_SID>\<Path_to_executable>

# TOOLS

Registry explorer of  Zimmerman
Documentation : https://aboutdfir.com/toolsandartifacts/windows/registry-explorer-recmd/

Download : https://ericzimmerman.github.io/#!index.md


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- Artefacts forensic : https://andreafortuna.org/2018/05/23/forensic-artifacts-evidences-of-program-execution-on-windows-systems/
- Source on BAM : https://dfir.ru/2020/04/08/bam-internals/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:49 | Page creation |