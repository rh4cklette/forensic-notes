---
tags:
  - EN
  - Finished
  - MissingSources
  - Persistance
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Introduction 

Search Order Hijacking is a technique where an attacker exploits the order in which Windows searches for executable files or DLLs.  
By placing a malicious file in a directory searched before the legitimate one, the attacker can achieve code execution.  
This technique is commonly used for privilege escalation or persistence.  
It often abuses writable directories or improperly configured applications.  
Detecting search order hijacking requires monitoring abnormal file locations and execution paths.

# Technique

When a program loads shared libraries (DLL) those are loaded in this order : 

![[Pasted image 20240716150113.png]]
<u><em>Figure 1: DLL search order</em></u>

```/!\``` The folder from which the application is launched isn't necessarily the current directory ```/!\```
It is possible in CMD to launch an executable that is in an other folder


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- 
### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 15:00 | Page creation |


