---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

When using PKINIT to obtain a TGT (Ticket Granting Ticket), the KDC (Key Distribution Center) includes in the ticket a PAC_CREDENTIAL_INFO structure containing the NTLM keys (i.e. LM and NT hashes) of the authenticating user.

This feature allows users to switch to NTLM authentications when remote servers don't support Kerberos, while still relying on an asymmetric Kerberos pre-authentication verification mechanism (i.e. PKINIT).

The NTLM keys will then be recoverable after a TGS-REQ through U2U, combined with S4U2self, which is a Service Ticket request made to the KDC where the user asks to authenticate to itself (i.e. S4U2self + User-to-User authentication).


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://github.com/ShutdownRepo/The-HackerRecipes/blob/master/ad/movement/kerberos/unpac-the-hash.md

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:23 | Page creation |