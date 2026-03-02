---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Introduction

Services on a compromised machine can be used by attackers to maintain persistence across reboots.  
They may allow the execution of malicious code with elevated privileges.  
Compromised services can be leveraged for lateral movement or remote command execution.  
Attackers may disguise malware as legitimate services to evade detection.  
Analyzing services helps investigators identify persistence mechanisms and abnormal behavior.

# Detection

- Event ID 4697 : Service Created
- Event ID 7045: A new service was installed in the system.

# Audit

Extension of the security system has to be audited. <br>Or in french : "extension du système de sécurité"

![[ServiceCreation.png]]
<u><em>Figure 1 : Audit needed for services in French</em></u>

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- MSDN

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 15:10 | Page creation |
| 2025-12-17 15:40 | Beautifying |

