---
tags:
  - Unfinished
  - MissingSources
  - EN
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Introduction

If we have a NT hash and NTLM authentication is available, it is possible to authenticate to a machine with this hash.

# Attack steps

Steal password hashes (An adversary can use multiple method to obtain password hashes, including DCSync and NTDS dump or NTDS extraction of hashes)

1.After the extraction of hashes the adversary can use Pass the Hash to any accessible ressource permitting NTLM authentication

2. Access other ressources with tools likes PSExec


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- 

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:24 | Page creation |