---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

One of the most obscure parts of the Kerberos protocol is delegation. And yet it is a very powerful and useful tool to let "agents" work on behalf of users w/o fully trusting them to do everything a user or an admin can.

So what is delegation ? Simply put is the ability to give a service a token that can be used on the user's behalf so that a service can act as if it were the user himself.


<b><u>Enter S4U constrained delegation</u></b>

Luckily for us Microsoft introduced a new type of "constrained" delegation normally referred to as S4U. This is an extension to the age old Kerberos delegation method and adds 2 flavors of delegation each depending on the KDC for authorization; they are called Service-for-User-to-Self (S4U2Self) and Servicefor-User-to-Proxy (S4U2Proxy).

<b><u>S4U2Self</u></b>

S4U2Self allows a service to get a ticket for itself on behalf of a user, or in other terms is allows to get a ticket as if a user requested it using it's krbtgt form a KDC and then contacted the service.

Note that the S4U2self process can be executed for _any_ user, and that target user’s password is not required. Also, the S4U2self process is only allowed if the requesting user has the TRUSTED_TO_AUTH_FOR_DELEGATION field set in their userAccountControl.

<b><u>S4U2Proxy</b></u>

S4U2Proxy is the actual method used to perform impersonation against a 3rd service. To use S4U2Proxy a service A that wants to authenticate to service B on behalf of user X, contacts the KDC using a ticket for A from user X (this could also be a ticket obtained through S4U2Self) and sends this ticket to the KDC as evidence that user X did in fact contact service A.

Bellow S4U2Self, followed by S4U2Proxy

![[Pasted image 20251120093934.png]]

<b><u>Protocol transition</u></b>



<b><u>So what does it looks like in practice ?</b></u>

Here’s how a service account configured with constrained delegation looks in the Active Directory GUI:
![[Pasted image 20251120103955.png]]

Here’s how that target object looks in PowerView:

![](https://miro.medium.com/v2/resize:fit:700/0*qEhdhQoqjLrSvBJH.png)

The field of interest is **msds-allowedtodelegateto**, but there’s also a modification to the account’s userAccountControl property. See **msds-allowedtodelegateto.useraccountcontrol** on the picture above.

And if a computer/user object has a userAccountControl value containing [TRUSTED_TO_AUTH_FOR_DELEGATION](https://learn.microsoft.com/en-gb/troubleshoot/windows-server/active-directory/useraccountcontrol-manipulate-account-properties)  then anyone who compromises that account can impersonate **any** user to the SPNs set in **msds-allowedtodelegateto**

Which means it is possible to impersonnate every user in SPNs HOST/PRIMARY.testlab.local etc.

<b><u>Going Further :</b></u> 

Check the following : https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-kerberos-constrained-delegation

The article tell us :  
Note that in this case we requested a TGS for the CIFS service, but we could also request additional TGS tickets with rubeus's switch for: HTTP (WinRM), LDAP (DCSync), HOST (PsExec shell), MSSQLSvc (DB admin rights).


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://ssimo.org/blog/id_011.html
- https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-sfu/1fb9caca-449f-4183-8f7a-1a5fc7e7290a

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:24 | Page creation |