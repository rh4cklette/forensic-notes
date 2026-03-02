---
tags:
  - Unfinished
  - FR
---
La délégation Kerberos permet à un service A d’accéder à un service B en se faisant passer pour un utilisateur. L’utilisateur “délégue” ses droits au service A.

<u>Unconstrained Delegation</u>

Ce type de délégation était le premier et il est toujours présent pour des raisons de rétrocompatibilité. Dans ce mode, l’utilisateur fournit un TGS au service A mais ce TGS contient un TGT pour l’utilisateur. Le service A peut alors utiliser ce TGT pour accéder au service B. Dans la pratique ceci lui permet d’accéder à n’importe quel service en se faisant passer pour l’utilisateur.

Dans le cas d’une Unconstrained Delegation, le serveur ou le compte de service qui se voit attribuer ce droit est en mesure de se faire passer pour l’utilisateur pour communiquer avec n’importe quel service sur n’importe quelle machine.

Cela signifie que maintenant, avec ces informations, le service peut demander n’importe quel ticket de service au nom de l’utilisateur. Je répète : il peut demander n’importe. quel. ticket de service. au nom de l’utilisateur

<u>Quelques notes:</u>

L’unconstrained delegation est configurée au niveau d’un serveur. Ainsi un utilisateur accédant à n’importe quel service de ce serveur enverrait son TGT.

Par défaut tous les contrôleurs de domaine ont l’Unconstrained Delegation.

Si l’utilisateur est dans le groupe “Protected Users” ou s’il a l’option “Account is sensitive and cannot be delegated” alors il n’enverra pas de TGT dans le TGS.

Diagramme final : 

![[Pasted image 20251120100925.png]]



How can you tel if a machine have unconstrained delegation ? 

This is actually pretty easy: search for any machine that has a [userAccountControl](https://msdn.microsoft.com/en-us/library/ms680832\(v=vs.85\).aspx) attribute containing [ADS_UF_TRUSTED_FOR_DELEGATION](https://msdn.microsoft.com/en-us/library/aa772300\(v=vs.85\).aspx). You can do this with an LDAP filter of ‘(userAccountControl:1.2.840.113556.1.4.803:=524288)’, which is what PowerView’s [**Get-DomainComputer**](http://powersploit.readthedocs.io/en/latest/Recon/Get-DomainComputer/#parameters) function does when passed the -Unconstrained flag:


![](https://miro.medium.com/v2/resize:fit:700/0*z5pmQS3BfH99p8rH.png)


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://beta.hackndo.com/unconstrained-delegation-attack/
- https://beta.hackndo.com/constrained-unconstrained-delegation/
- https://ruuand.github.io/Kerberos_Delegation/
- https://harmj0y.medium.com/s4u2pwnage-36efe1a2777c

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:26 | Page creation |