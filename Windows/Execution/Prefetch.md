---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

## Introduction

This artifact is available only on <u>desktop</u> versions of Windows and gives information on executed programs on a machine since Windows 8.

At the first execution of a program from a location, a prefetch file is created.
This file give information on the last 8 executions.

Data that are to be expected in Prefetch : 

• Executable’s name
• Eight character hash of the executable path.
• The path of the executable file
• Creation, modified, and accessed timestamp of executable
• Run count (Number of time the application has been executed)
• Last run time
• The timestamp for the last 8 run time (1 last run time and other 7 other last run times)
• Volume information
• File Referenced by the executable
• Directories referenced by the executable

There are some technical details to know regarding the hash of the execution path if we want to parse it manualy. In any case, Zimmerman tools indicate the path if needed [3].

### Location

- ``C:\Windows\Prefetch

Registry key used to check if prefetch are activated : 

- ``HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management\PrefetchParameters

Default value == 3 => Prefetch enabled for application launch and boot .

### Tools

- <u>PECMD from Zimmerman :</u> https://ericzimmerman.github.io/#!index.md
- <u>Documentation :</u> https://github.com/EricZimmerman/PECmd


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- SANS Prefetch : https://www.sans.org/blog/device-profiling-with-windows-prefetch/
- Forensic view of prefetch : https://www.hackingarticles.in/forensic-investigation-prefetch-file/
- Hexacorn Informations on prefetch path hash : https://www.hexacorn.com/blog/2012/06/13/prefetch-hash-calculator-a-hash-lookup-table-xpvistaw7w2k3w2k8/
- https://www.ghacks.net/2008/01/13/enableprefetcher-in-prefetchparameters/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:52 | Page creation |