---
tags:
  - EN
  - Finished
---

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Introduction

MRU are the Most Recently Used files, programs, URL ... opened.
They depend principaly on what we're searching and where.

A non exhaustive list of the MRU is available under : 

#### <u>Recent opened Programs/Files/URLs </u>

This key maintains a list of recently opened or saved files via Windows Explorer-style dialog boxes (Open/Save dialog box).

- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSaveMRU

Whenever a new entry is added to OpenSaveMRU key, registry value is created or updated in
- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedMRU

#### <u>List of files recently opened directly from Windows Explorer are stored into </u>
- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs

#### <u>List of entries executed using the Start>Run command are mantained in this key: </u>
- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU

#### <u>UserAssist </u>
- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist

This key contains two GUID subkeys: each subkey maintains a list of system objects such as program, shortcut, and control panel applets that a user has accessed.

#### <u>Office MRU </u>
- ``HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\<Office_Version>\<Office_product>\User MRU\

The subkeys contain a list of Offices documents opened by a user (Excel, Word etc)


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://andreafortuna.org/2017/10/18/windows-registry-in-forensic-analysis/
- https://www.researchgate.net/publication/221352819_Temporal_Analysis_of_Windows_MRU_Registry_Keys

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:52 | Page creation |
