---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

If you’re asking yourself “so what” or skipped ahead to this section, we can think of a few ways that the S4U extensions can come into play on a pentest.

The first is to enumerate all computers and users with a non-null **msds-allowedtodelegateto** field set. This can be done easily with PowerView’s **-TrustedToAuth** flag for **Get-DomainUser/Get-DomainComputer**:

![](https://miro.medium.com/v2/resize:fit:700/0*Kx-EcHZVVjeuaOee.png)

Now, remember that a machine or user account with a SPN set under **msds-allowedtodelegateto** can pretend to be any user they want to the target service SPN. So if you’re able to compromise one of these accounts, you can spoof elevated access to the target SPN. For the HOST SPN this allows complete remote takeover. For a MSSQLSvc SPN this would allow DBA rights. A CIFS SPN would allow complete remote file access. A HTTP SPN it would likely allow for the takeover of the remote webservice, and LDAP [allows for DCSync](https://twitter.com/gentilkiwi/status/806643377278173185) HTTP/SQL service accounts, even if they aren’t elevated admin on the target, can also possibly be abused with [Rotten Potato](https://foxglovesecurity.com/2016/09/26/rotten-potato-privilege-escalation-from-service-accounts-to-system/) to elevate rights to SYSTEM (though I haven’t tested this personally).

For the SPNs from an other source : 

-  TGS tickets with rubeus's switch for: HTTP (WinRM), LDAP (DCSync), HOST (PsExec shell), MSSQLSvc (DB admin rights).


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://harmj0y.medium.com/s4u2pwnage-36efe1a2777c
- https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-kerberos-constrained-delegation


### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-11-20 11:39 | Page creation |


