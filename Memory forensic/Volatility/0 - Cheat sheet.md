---
tags:
  - EN
  - Unfinished
---

<div style="text-align: center;"><font color="#FF2B2B" style="background-color:#000000;" size=+3> TLP RED </font></div>

# Volatility cheatsheet

## Introduction
Volatility is THE memory analyzer. From a RAM dump it allows to harvest and parse what was existing at the moment of the RAM dump

Problem : It is realy complete. A bit too much.

To follow what's next, it can be interesting to have the SANS poster "memory_commands.pdf" alongside this document.

### Mini starting guide

First, it is needed to get the profile associated with the OS : 

```volatility_standalone.exe -f <imagePath> imageinfo```

Then, to get the most precise image possible corresponding to the analyzed machine : 

![[Volatility_profile.png]]

Once the profile is ready, it's possible to charge it during the analysis.
The starting point is often to list the processes :

```Volatility_standalone.exe -f <imagePath> --profile=<profile_name> pslist```

Then listing them in a tree view, to detect processes that don't have a normal parent for example : 

```Volatility_standalone.exe -f <imagePath> --profile=<profile_name> pstree```

![[Volatility_processes.png]]

Once a suspect process has been found (here svchost that uses WMI [1]), it is possible to launch investigations, by listing injected DLLs for example : 

```Volatility_standalone.exe -f <imagePath> --profile=<profil_name> dlllist -p <list_PID>604,144,1276```

It is then possible to dump a process that seems malicious with the following command : 

```Volatility_standalone.exe -f <imagePath> --profile=<profile_name> procdump --dump-dir <output_folder> -p <list_PID>```

Then it is possible to do reverse engineering on the content of svchost if we know how to do it. Else it is possible to identify IOCs from an incident in the memory of the process.

### Tools 

Volatility 2 : ```http://downloads.volatilityfoundation.org/releases/2.6/volatility_2.6_win64_standalone.zip``` 

### Ressources

1 : Page on WMI Forensics : WMI Forensics
2 : Posters "memory methode" et "memory commands" du SANS.

### MINDMAPS

Volatility_mindmap_1 : Mindmap Memory Hunting

![[Volatility_mindmap_1.png]]

Volatility_mindmap_2 : Volatility and associated modules

![[volatility_mindmap_2 1.png]]




<div style="text-align: center;"><font color="#FF2B2B" style="background-color:#000000;" size=+3> TLP RED </font></div>

### Resources

The sources used for the creation of this article are the following :
- Couldn't find the source of those image anymore, if you are the author and would like to be credited please ping.

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-06 09:40 | Page creation |
