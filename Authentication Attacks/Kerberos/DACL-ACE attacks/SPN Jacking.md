---
tags:
  - EN
  - Unfinished
  - FR
---
Rappel sur ce qu'est un SPN : 

Un SPN est **le nom par lequel un client identifie de manière unique une instance d'un service**
A lot more info on what a SPN is : https://beta.hackndo.com/service-principal-name-spn/

Voir aussi [[Constrained]] AND Delegation for a reminder before reading this post.

This attack combines [Kerberos Constrained delegation abuse](https://www.thehacker.recipes/ad/movement/kerberos/delegations/constrained) and [DACL abuse](https://www.thehacker.recipes/ad/movement/dacl/).


A service configured for Kerberos Constrained Delegation (KCD) can impersonate users on a set of services. The "set of services" is specified in the constrained delegation configuration. It is a list of SPNs (Service Principal Names) written in the `msDS-AllowedToDelegateTo` attribute of the KCD service's object.


Since KCD allows for impersonation, the attacker can also impersonate users (e.g. domain admins) on the target services. Depending on the SPNs, or if it's possible to [modify it](https://www.thehacker.recipes/ad/movement/kerberos/ptt#modifying-the-spn), the attacker could also gain admin access to the server the "listed SPN" belongs to.

If attacker is able to move a "listed SPN" from the original object to the another one, he could be able to compromise it. This is called SPN-jacking and it was intially discovered and explaine by [Elad Shamir](https://twitter.com/elad_shamir) in [this post](https://www.semperis.com/blog/spn-jacking-an-edge-case-in-writespn-abuse/).

1. In order to "move the SPN", the attacker must have the right to edit the target object's `ServicePrincipalName` attribute (i.e. `GenericAll`, `GenericWrite` over the object or`WriteProperty` over the attribute (called `WriteSPN` [since BloodHound 4.1](https://posts.specterops.io/introducing-bloodhound-4-1-the-three-headed-hound-be3c4a808146)), etc.).
2. If the "listed SPN" already belongs to an object, it must be removed from it first. This would require the same privileges (`GenericAll`, `GenericWrite`, etc.) over the SPN owner as well (_a.k.a. "Live SPN-jacking"_). Else, the SPN can be simply be added to the target object (_a.k.a. "Ghost SPN-jacking"_).


Case where the SPN does not exist anymore but is still listed, you can create it on your side => Ghost SPN-Jacking
For Ghost jacking : Write over the SPN Owner

If SPN still exist on an other object, you have to delete it and create it on the object you have control over : Live SPN-Jacking
For Live SPN-Jacking : Remove on the object + Write over the SPN Owner



Use-Case : 

Supposons qu'un attaquant compromette un compte configuré pour la délégation restreinte, mais qu'il ne dispose pas du privilège **SeEnableDelegation** (donc S4U2Self ou S4U2Proxy ??) L'attaquant ne pourra pas modifier les contraintes (**msDS-AllowedToDelegateTo**). Cependant, si l'attaquant dispose des droits **WriteSPN** sur le compte associé au SPN cible, ainsi que sur le compte d'un autre ordinateur/service, il peut temporairement détourner le SPN (technique appelée **SPN-jacking**), l'affecter à l'autre ordinateur/serveur, et effectuer une attaque S4U complète pour le compromettre.






<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://www.thehacker.recipes/ad/movement/kerberos/spn-jacking
- https://www.semperis.com/fr/blog/spn-jacking-an-edge-case-in-writespn-abuse/
- https://beta.hackndo.com/service-principal-name-spn/
-

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-11-20 09:25 | Page creation |