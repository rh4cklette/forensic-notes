---
tags:
  - EN
  - Unfinished
---


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# AutomaticDestinations

The Windows 7 task bar (Jump List) is engineered to allow users to “jump” or access items they have frequently or recently used quickly and easily. This functionality cannot only include recent media files; it must also include recent tasks. 
The data stored in the AutomaticDestinations folder will each have a unique file prepended with the AppID of the associated application. 


### Location

- ``C:\%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations

### Tools

- JLExplorer from Zimmerman
- JLECmd from Zimmerman 


<u>Download :</u> https://ericzimmerman.github.io/#!index.md
<u>Documentation :</u> https://github.com/EricZimmerman/JLECmd


# Link Files

Windows uses the folder stores LNK files associated with files a user has recently accessed, typically by double-clicking on it in a Windows Explorer window.
If the file is reopened, it will be overwritten with the latest file access regardless of whether the file exists in a different directory.
In Windows 10 and later, Microsoft started adding the extension of the LNK file and preventing supersecretfile.xlsx from overwriting the LNK file for supersecretfile.txt.
Persist even if file itself was deleted, cannot be seen in explorer even if Hidden Items is ticked [1].


![[lnkfiles_1.png]]
<u><em>Figure 1 – Recent folder viewed in Windows Explorer</em></u>

![[lnkfiles_2.png]]
<u><em>Figure 2 – Recent folder viewed via command line</em></u>

### Location

- ``C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Recent

### Tools 

We can parse LNK manualy when right-clicking on it in the GUI > Property
Or use JLECmd
### LECmd de Zimmerman

<u>Download :</u> https://ericzimmerman.github.io/#!index.md
<u>Documentation :</u> https://github.com/EricZimmerman/LECmd


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- Informations on fsecure : https://frsecure.com/blog/windows-forensics-execution/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 17:08 | Page creation |
