---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

This document contains a list of interesting Event ID in the context of DIFR. A part of those logs depend of the logs policy in place and may not be available during your engagement.
### <u>Security Event ID</u>
#### <u>Connect</u>

4624 An account was successfully logged on. (See Logon Type Codes)
4625 An account failed to log on.
4634 An account was logged off.
4647 User initiated logoff. (In place of 4634 for Interactive and RemoteInteractive logons)

#### <u>Special logon</u>

4648 A logon was attempted using explicit credentials. (RunAs)
4672 Special privileges assigned to new logon. (Admin login)

#### <u>Kerberos / NTLM</u>

4768 A Kerberos authentication ticket (TGT) was requested.
4769 A Kerberos service ticket was requested.
4770 kerberos ticket was renewed
4771 Kerberos pre-authentication failed.
4776 Computer tried to validate authentication information (on DC for domain acc, on local computer for local accounts) => NTLM Only

#### <u>User accounts</u>

4720 A user account was created (for both local and domain).
4722 A user account was enabled.
4728 User added to security global group
4732 User added to security enabled local group

#### <u>Task and enumeration</u>

<u>Task</u>

4688 A new process has been created. (If audited; some Windows processes logged by default)
4697 A new service
4698 A scheduled task was created. (If audited)
4798 A user's local group membership was enumerated.
4799 A security-enabled local group membership was enumerated.

<u>Share enumeration </u>

5140 A network share object was accessed.
5145 A network share object was checked to see whether client can be granted desired access.

#### <u>Services</u>

7045 in system
7036 in system

#### <u>Logs Clear</u>

1102 The audit log was cleared. (Security)

#### Logon Type Codes

<u>Type Description</u>

2 Console
3 Network
4 Batch (Scheduled Tasks)
5 Windows Services
7 Screen Lock/Unlock
8 Network (Cleartext Logon)
9 Alternate Credentials Specified (RunAs ou Overpass the hash sur machine source)
10 Remote Interactive (RDP)
11 Cached Credentials (e.g., Offline DC)
12 Cached Remote Interactive (RDP, similar to Type 10)
13 Cached Unlock (Similar to Type 7)

### <u>System Event IDs of Interest</u>

7045 A new service was installed in the system. (4697 in Security)
7034 The x service terminated unexpectedly. It has done this y time(s).
7009 A timeout was reached (x milliseconds) while waiting for the y service to connect.

104 The x log file was cleared. (Will show System, Application, and other logs cleared)

### <u>Application Event IDs of Interest</u>


1000 Application Error
1002 Application Hang

### <u>RDP Event IDs of Interest</u>

Microsoft-Windows-TerminalServices-LocalSessionManager/Operational

21 Remote Desktop Services: Session logon succeeded:
22 Remote Desktop Services: Shell start notification received
23 Remote Desktop Services: Session logoff succeeded:
24 Remote Desktop Services: Session has been disconnected:
25 Remote Desktop Services: Session reconnection succeeded:

### <u>Scheduled Tasks</u>

Microsoft-Windows-TaskScheduler/Operational Event IDs of Interest

100 Task Scheduler started the x instance of the y task for user z.
102 Task Scheduler successfully finished the x instance of the y task for user z.
106 The user x registered the Task Scheduler task y. (New Scheduled Task)
141 User x deleted Task Scheduler task y.

### <u>Powershell</u>

Microsoft-Windows-PowerShell/Operational Event IDs of Interest

4104 Creating Scriptblock text (1 of 1): (Scriptblock Logging)

### <u>Bonus</u>

For NTDS dump (using ESENT) : 

325 DB created in Application
327 DB attached in Application
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- MSDN
- RDP Bible : https://ponderthebits.com/2018/02/windows-rdp-related-event-logs-identification-tracking-andinvestigation/
#### Log: Microsoft-Windows-TerminalServices-LocalSessionManager/Operational
### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-12 21:23 | Page creation |
| 2024-11-13 16:32 | TLP Modification + licence |

