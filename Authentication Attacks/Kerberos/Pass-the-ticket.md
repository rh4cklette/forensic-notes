---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
### Principle

In a pass-the-ticket attack, an attacker extracts a Kerberos Ticket Granting Ticket (TGT) from LSASS memory on a system and then uses this valid ticket on another system to request Kerberos service tickets (TGS) to gain access to network resources.

Steal TGT => Inject it in session on other machine => Get TGS

One primary difference between pass-the-hash and pass-the-ticket is that Kerberos TGT tickets expire (10 hours by default), whereas NTLM hashes change only when the user changes their password. So a TGT ticket must be used within its lifetime, or it can be renewed for a longer period of time (7 days).

#### <u>Detection</u>

List connected users on the system with their session ID.
Use ```klist -li``` to see the TGT associated with each session.

If the username does not correspond in the ticket /!\ Pass-the-ticket  detected/!\
### Ressource

The sources used for the creation of this article are the following :
- https://book.hacktricks.xyz/windows-hardening/active-directory-methodology/pass-theticket

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:21 | Page creation |
