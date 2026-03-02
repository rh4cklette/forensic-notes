---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FF2B2B" style="background-color:#000000;" size=+3> TLP RED </font></div>

L'objectif est de détecter les sets d'attaques suivants : 

DACL Abuse mind-map : 
![[Pasted image 20251120150040.png]]

Rappel sur les droits / actions qu'ils permettent : 
- **GenericAll** - full rights to the object (add users to a group or reset user's password)
- **GenericWrite** - update object's attributes (i.e logon script)
- **WriteOwner** - change object owner to attacker controlled user take over the object
- **WriteDACL** - modify object's ACEs and give attacker full control right over the object
- **AllExtendedRights** - ability to add user to a group or reset password
- **ForceChangePassword** - ability to change user's password
- **Self (Self-Membership)** - ability to add yourself to a group

## Add Member to privileged group

Le AddMember est normalement déjà implémenté chez ITR.

## ForceChangePassword

[ForceChangePassword](https://www.thehacker.recipes/ad/movement/dacl/forcechangepassword) is the section referenced within the Hacker Recipes that defines a collection of permissions that allow for control over changing the password of another user/object. More specifically, the permissions referenced are:

- GenericAll
- AllExtendedRights
- User-Force-Change-Password

Detecter l'attaque ForceChangePassword (https://www.thehacker.recipes/ad/movement/dacl/forcechangepassword)
Nécessite les SACL tel qu'indiqué ici : https://trustedsec.com/blog/a-hitchhackers-guide-to-dacl-based-detections-part-1-a
Et la règle indiquée ici est à tester / challenger : https://trustedsec.com/blog/a-hitch-hackers-guide-to-dacl-based-detections-part-1b



Est-ce que gMSA et LAPS sont utilisés côté ITR ? 

GrantOwnership : 

[GrantOwnerShip](https://www.thehacker.recipes/ad/movement/dacl/grant-ownership) (or Generic Write on User) is the section referenced within the Hacker Recipes that defines a collection of permissions that allows for more general control of another user/object. More specifically, the permissions referenced are:

<div style="text-align: center;"><font color="#FF2B2B" style="background-color:#000000;" size=+3> TLP RED </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://trustedsec.com/blog/a-hitch-hackers-guide-to-dacl-based-detections-part-1b
- Configuration of SACL + events : https://trustedsec.com/blog/a-hitchhackers-guide-to-dacl-based-detections-part-1-a
- https://www.ired.team/offensive-security-experiments/active-directory-kerberos-abuse/abusing-active-directory-acls-aces

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-11-20 14:58 | Page creation |