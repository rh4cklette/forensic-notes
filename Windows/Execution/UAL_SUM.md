---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Introduction

Forensic artefact that contains data relative to user connections, when OS versions changed, number of access etc.

Enable by default since Windows Server 2012.


![[UAL_picture1.png]]
<u><em>Figure 1 : Exemple of data that can be harvested</em></u>
### Informations to know : 

So it is interesting to use tools like RawCopy64 to copy the files.
It is then necessary to repare the hives that were dirty since they've been extracted forcefully while in use.

Reparations are made with the tool esentutl.exe from Microsoft.
The command is the following : 
```esentutl.exe /p <databasename>.mdb```

### Tools

- SumEcm from Zimmerman : https://github.com/EricZimmerman/Sum
- Kstrike from BriMor Labs : https://github.com/brimorlabs/KStrike
- RawCopy64 : https://github.com/jschicht/RawCopy
- Esentult.exe : https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh875546(v=ws.11)


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- Youtube on the channel of 13Cubed : https://www.youtube.com/watch?v=rVHKXUXhhWA

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:59 | Page creation |
