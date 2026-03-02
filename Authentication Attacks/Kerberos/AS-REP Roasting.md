---
tags:
  - Unfinished
  - FR
  - MissingSources
---
<b><u>Explanations</u></b>

Lors de la demande AS-REQ (première étape lors de l'authentification Kerberos), il est normalement nécessaire de joindre des informations de pré-authentification.

Microsoft propose un paramètre Do not require Kerberos preauthentification pour des comptes qui n'ont pas besoin de l'utiliser.

<b><u>Attack</u></b>

En forgeant un AS-REQ pour un user qui ne nécessite pas la préauthentification, on peut downgrade le chiffrement du TGT en RC4 (au lieu d'AES256) afin de casser rapidement le mot de passe hors ligne et de récupérer son password.


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- If you are the source of this article please ping me and I'll add you here.

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:18 | Page creation |