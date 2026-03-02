---
tags:
  - EN
  - Finished
  - Persistance
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Introduction

Tasks crated by schtasks.exe are located in the folder ```C:\Windows\System32\Tasks``` and those created by at.exe are in ```C:\Windows\Tasks``` (they are called job and their extension is .job)

Creation of XML files in the ```%SystemRoot%\System32\Tasks``` folder.
If enabled, then Task creation and modification can be seen in ```Microsoft-Windows-TaskScheduler/Operational``` event log.

# Localization

<u>Windows logs</u>

<b>Security.evtx</b>

4624 Logon Type 3 Source IP/Logon User Name  
4672 Logon User Name Logon by a user with administrative rights Requirement for accessing default shares such as C$ and ADMIN$  
4698 – Scheduled task created  
4702 – Scheduled task updated  
4699 – Scheduled task deleted  
4700/4701 – Scheduled task enabled/disabled  
  
<b>Microsoft-Windows-Task Scheduler%4Operational.evtx</b>
106 – Scheduled task created  
140 – Scheduled task updated  
141 – Scheduled task deleted  
200/201 – Scheduled task executed/complete

#### <u>Going further</u>


```schtasks.exe /CREATE /SC ONEVENT /EC application /mo *[System/EventID=777] /f /TN run /TR "calc.exe" & EVENTCREATE /ID 777 /L APPLICATION /T INFORMATION /SO DummyEvent /D "Initiatescheduled task." &  schtasks.exe /DELETE /TN run /f```

The above command leverages schtasks.exe to create a one-time task to call a portable executable. In this case the executable is called calc.exe. The trigger for this task is contingent on the creation of a Windows event with EventID of 777. The command then creates a dummy event to trigger the task and deletes the task from the task scheduler. This peculiar application of tasking logic results in the portable executable being executed as a child process of taskhostsw.exe which is a signed Windows binary.

See resource about Tarrask malware andd Serpent backdoor bellow

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
### Ressource

The sources used for the creation of this article are the following :
- General information on scheduled tasks : https://nasbench.medium.com/a-deep-dive-into-windows-scheduled-tasks-and-the-processes-running-them-218d1eed4cce
- Forensic Scheduled tasks : https://www.forensafe.com/blogs/taskschd.html
- https://www.microsoft.com/security/blog/2022/04/12/tarrask-malware-uses-scheduled-tasks-for-defense-evasion/
- https://www.proofpoint.com/us/blog/threat-insight/serpent-no-swiping-new-backdoor-targets-french-entities-unique-attack-chain

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 14:48 | Page creation |