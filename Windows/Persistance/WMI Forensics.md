---
tags:
  - EN
  - Finished
  - Persistance
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
## Introduction 

WMI is a built-in tool that is normal in a Windows environments. Admins, installer scripts, and monitoring software can all use it legitimately (to query if a disk is SSD, free disk space etc) 

However WMI can be used in post-exploitation phases \[1\].
Previously it was launched through the legitimate wmic.exe, little by little it was replaced by Powershell which natively includes WMI \[2\].

![[Pasted image 20240716150419.png]]
<u><em>Figure 1 : General terms used in WMI </em></u>

## Processus

WMI CommandLine Event Consumers kick off a new WmiPrvSE process to run an executable or PowerShell script. <br><u>WmiPrvSE.exe</u> process (WMI Provider Host) is responsible for running WMI commands on a remote (target) system
<br>
If a <u>wsmprovhost.exe</u> process is identified on a system, it indicates PowerShell remoting activity.
<br><u>wmic.exe</u> - Commandline tool for interacting with WMI locally and for remote systems
Scrcons.exe (literally named script consumers .exe) is the parent of any ActiveScript consumers such as VBScript or Jscript (it is also a rare process, making it a great candidate for detections)

## Logs 

It is possible to determine the actions done with WMI in the following logs :

**Security events** 
- 4688 for any of the processes listed above

**Microsoft-Windows-WMI-Activity**
- 5860 for temporary event consumer creation
- 5861 for permanent event consumer creation
<br>
If the machine is powered-on and it is possible to launch scripts, you can launch the following commands :


    Get-WmiObject -Class __FilterToConsumerBinding -Namespace root\subscription 
    Get-WmiObject -Class __EventFilter -Namespace root\subscription
    Get-WmiObject -Class __EventConsumer -Namespace root\subscription
    
### <span style="text-decoration:underline">Today I'm LUUCKKYYY</span>

In case of extreme luck (so extreme that the poster "Find evil" of the SANS doesn't talk about it), the WMI troubleshooting can be activated.
It is jackpot, and the WMI files \.log are generated in ```C:\Users\<username>\AppData\Local\Temp\```
 
The log files are of this form : 

![[Pasted image 20240716150755.png]]
<u><em>Figure 2 : Exemple of WMI log file </em></u>

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- WMI Forensics : https://netsecninja.github.io/dfir-notes/wmi-forensics/
- WMI through Powershell : https://docs.microsoft.com/fr-fr/powershell/scripting/learn/ps101/07-working-with-wmi?view=powershell-7.2 
- Informations WMI : https://www.sans.org/blog/investigating-wmi-attacks/ 
- MISC-091 on WMI : https://connect.ed-diamond.com/MISC/MISC-091/Detecter-la-persistance-WMI 
- Troubleshooting WMI : https://docs.microsoft.com/fr-fr/troubleshoot/windows-client/application-management/enable-windows-installer-logging 

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 15:08 | Page creation |
