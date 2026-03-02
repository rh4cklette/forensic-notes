---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

## Introduction

Shims are a small addons that serve for retro-compatibility purposes. This can allow to have a timeline of programs executed, as executables are listed in the Shimcache when it is checked if it needs compatibility related actions, not when it is actually shimmed [1].
![[Pasted image 20240716164701.png]]
<u><em>Figure 1 : Execution with and without Shim </em></u>
### Data available in the artifact

List of file metadata from executables that were recently executed, and thus examined for shimming, or executables examined for “the need to shim” but not executed. 
=> This can be verified with the tag "Execution" at "True/False" 

(side note : DLLs are sometimes listed with the wrong flag)

### Important points

- AppCompatCache is written only at system power off. If in parsing AppCompatCache there are no data that are interesting, check the machine uptime.
- When a machine was not rebooted since a long time, as Shimcache was not written on disk, there is a Volatility plugging made to grab the Shimcache that would be written at next reboot
- Be carefull of TimeStomp. To detect TimeStomp, ShimCache lists the files in order of execution (the most recent on top)

### Exemple

![[Pasted image 20240716164745.png]]
<u><em>Figure 2 : Output of shimcachemem plugin in Volatility [2]</em></u>

We can deduce that a.exe is the last executable launched, followed by SearchFilterHost.exe

### Localisation

- ```HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache\AppCompatCache```

(See AutoStart Locations / Registry Forensic to understand how to get the hive and process it)
### Going further

See NTFS file fields, execution and detection of timestomping


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- Information on Shimcache : https://andreafortuna.org/2017/10/16/amcache-and-shimcache-in-forensic-analysis/
- Exemple of Shimcache use : https://bromiley.medium.com/windows-wednesday-shim-cache-1997ba8b13e7

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:48 | Page creation |
