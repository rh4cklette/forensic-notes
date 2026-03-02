---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

Ransomware compromise on VCenter infrastructure.
The scenario is the following : 

- An attacker manages to get inside the network
- The VCenter is joined to the Active Directory
- The multiple ESXi are not joined to AD.
- The attackers fails to connect to the ESXi as he doesn't have the accounts used for SSH and bruteforce failed.
- So he connects to the VCenter, joins the ESX to the domain.
- Adds his user in the "ESX Admins" group, so he does have the rights to connect through SSH to the ESXi
- Then he connects with SSH to the ESXi and can upload his ransomware and ransom the ESXi
- The ransom stops the VM to have access to the locks on the VM files and then ecrypt the VMs files (VMDK, datastore etc).

The timeline is the following : 

![[Pasted image 20240712155053.png]]


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- DFIR4VSphere https://www.youtube.com/watch?v=2EXtHr0NDPk

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-26 15:28 | Page creation |
