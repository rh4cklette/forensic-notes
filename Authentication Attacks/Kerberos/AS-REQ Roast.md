---
tags:
  - Unfinished
  - MissingSources
  - EN
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

This page is clearly inspired from : https://www.thehacker.recipes/ad/movement/kerberos/asreqroast
#### Principle

In Kerberos a ST (Service Ticket) can be obtained by presenting a TGT (Ticket Granting Ticket). That prior TGT can be obtained by validating a first step named "pre-authentication" (except if that requirement is explicitly removed for some accounts, making them vulnerable to ASREProast).

The pre-authentication requires the requesting user to supply its secret key (DES, RC4, AES128 or AES256) derived from the user password.

Technically, when asking the KDC (Key Distribution Center) for a TGT (Ticket Granting Ticket), the requesting user needs to validate pre-authentication by sending a timestamp encrypted with it's own credentials in an AS_REQ message.

It ensures the user is requesting a TGT for himself.

<u><b>Attack</b></u>

When attackers obtain a man-in-the-middle position, they are sometimes able to capture preauthentication messages, including the encrypted timestamps. Attackers can try to crack those encrypted timestamps to retrieve the user's password.

<b><u>How To</u></b>

The only thing you have to do is to intercept the first packet in the kerberos authentication process, called AS-REQ.

#### <u>Conclusion</u>

This technique is <b>similar to ASREProasting but doesn't rely on a misconfiguration</b>. It relies instead on an attacker successfully obtain a powerful enough man-in-the-middle position (i.e. ARP poisoning, ICMP redirect, DHCPv6 spoofing)


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://www.thehacker.recipes/ad/movement/kerberos/asreqroast

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:16 | Page creation |
