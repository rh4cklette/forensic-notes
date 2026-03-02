---
tags:
  - EN
  - Finished
  - MissingSources
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

## Introduction

There are user assist key that contain data interesting from a forensic point of vue.

The keys are located in : 
- ``NTUSER.DAT

And inside :  
- ``SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\

Unlike prefetch files, UserAssist data will include information on whether an application was run from a shortcut (LNK file) or directly from the executable.

Data are Rot13 but Zimmerman tools can be used to decypher them.

### Tools
- RegRipper : https://github.com/keydet89/RegRipper4.0
- Registry Explorer : https://ericzimmerman.github.io/#!index.md

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- Source ?

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 17:03 | Page creation |