---
tags:
  - FR
  - Unfinished
  - EN
---
This page is clearly a modified copy of : https://beta.hackndo.com/ntlm-relay/
# Introduction

Windows New Technology LAN Manager (NTLM) is a suite of security protocols offered by Microsoft to authenticate users’ identity and protect the integrity and confidentiality of their activity. At its core, NTLM is a single sign on (SSO) tool that relies on a challenge-response protocol to confirm the user without requiring them to submit a password.

Se repose sur l'authentification NTLM, le challenge response se passe en trois parties qui sont définies comme suit :

1. D’abord le client indique au serveur qu’il veut s’authentifier.
2. Le serveur répond alors avec un défi, ou un challenge, qui n’est rien d’autre qu’une suite aléatoire de caractères.
3. Le client chiffre ce défi avec son secret, et renvoie le résultat au serveur, c’est sa réponse.

![[Pasted image 20240715170108.png]]

L’intérêt de cet échange, c’est que le secret de l’utilisateur ne transite jamais sur le réseau. C’est ce qu’on appelle une preuve à divulgation nulle de connaissance.

Une fois que le client est authentifié, il obtient alors une session, durant laquelle le client va pouvoir faire des actions, comme par exemple accéder aux partages réseau, accéder à l'annuaire LDAP, serveur HTTP etc.

Il existe deux version de NTLM, qui seront détaillées ci-dessous mais la V1 est à bannir.

#### <u>Le fonctionnement</u>

La couche applicative (HTTP, SMB, SQL) est complètement indépendante de la couche d'authentification (NTLM, Kerberos), ce qui signifie que l'authentification NTLM est possible à travers de nombreuses couches applicatives.

Cela est mis en place au travers de SSPI et NTLMSSP mais ne sera pas décrit en détails ici. Plus de détails ici : https://beta.hackndo.com/ntlm-relay/

The following part is directly copied from Hackndo website.

<u>Les problèmes, NTLM relai :</u>

On voit bien qu'il est possible de faire un relai via le système d'authentification précédent via attaque par homme du milieu :
![[Pasted image 20240715170204.png]]
Figure 1 : Relai NTLM

Du point de vue du serveur il s'est passé ça :
![[Pasted image 20240715170215.png]]
Figure 2 : NTLM relai vu depuis le serveur

Mais étant donné que l'authentification est agnostique du protocole, il est également possible de faire des relais via différents protocoles, c'est ce qu'on appelle un relai-cross-protocole

![[Pasted image 20240715170227.png]]

Alors comment fix les problèmes de relai NTLM ?

On va mettre en place des systèmes de signature de message, ce qui limitera les possibilités de l'attaquant.

Les échanges se présenteront donc comme ça :

![[Pasted image 20240715170246.png]]
Figure 3 : Echanges avec signature des échanges

Si l'attaquant essaie de faire la technique de l'homme du milieu :
Figure 4 : Echec de la technique de l'homme du milieu
![[Pasted image 20240715170303.png]]
Figure 4 : Echec de la technique de l'homme du milieu

Ne connaissant pas le secret du client, l'attaquant n'a pas la possibilité de signer les échanges. Il n'a pas la possibilité d'effectuer de relay puisque si la signature est obligatoire, le serveur va kill les échanges.

A noter que l'implémentation du système de signature dépends du protocole applicatif utilisé.


En fonction des différentes valeurs de signatures acceptées par le client et le serveur on peut avoir des communications non signées, mais ceci est hors scope de ce document.

Pour plus d'infos, pour SMB CF : https://beta.hackndo.com/assets/uploads/2020/03/ntlm_signing_table.png Pour LDAP cf : https://beta.hackndo.com/assets/uploads/2020/03/ntlm_ldap_signing_table.png

<b><u>Les problèmes avec le cross relai protocol</u></b>

On a vu qu'il est possible faire du cross relai, alors si par exemple nous avons un attaquant qui essaie de relayer du SMB qui nécessite une signature, il pourrait le faire passer par du LDAP et dropper le drapeau correspondant au fait que tout doit être signé.

MAIS, il y a une signature au niveau NTLM, appelé le MIC (Message Integrity Code) qui dépends du secret du client, il n'est donc pas possible sans connaitre le secret du client de faire du cross relai en utilisant des flux qui doivent être signés.

Et si l'attaquant essaie de faire sauter le MIC, d'autres mécanismes de vérification entrent en jeux et tout pète.

A noter que NTLMv1 ne supporte pas de système de vérification du MIC, donc on peut faire sauter le système de signature.

<b><u>Pourquoi péter le hash NetNTLMv1 ou NetNTLMv2 ?</u></b>

Pour obtenir le mot de passe en clair de l'utilisateur. En général c'est ce qu'on fait quand tout doit être signé et que l'attaquant n'a pas d'autres choix.

<u><b>Pour plus de détails</b></u>

<u>NTLM Authentication Process in details (done in the 3 communications process)</u>

NTLM authentication typically follows the following step-by-step process:

• The user shares their username, password and domain name with the client.
The client develops a scrambled version of the password — or hash — and deletes the full password.
• The client passes a plain text version of the username to the relevant server.
• The server replies to the client with a challenge, which is a 16-byte random number.
• The server replies to the client with a challenge, which is a 16-byte random number.
• In response, the client sends the challenge encrypted by the hash of the user’s password.
• The server then sends the challenge, response and username to the domain controller (DC).
• The DC retrieves the user’s password from the database and uses it to encrypt the challenge.

The DC then compares the encrypted challenge and client response. If these two pieces match, then the user is authenticated and access is granted.

### <u>Key Takeaways</u>

NTLM authentification en trois temps :
Negotiate, Challenge, Authenticate

Protocole legacy, préférer l'utilisation de Kerberos 
Possibilité de relayer l'authentification
Signature afin d'éviter le relai, nécessite d'être géré par protocole.
Casser un hash NetNTLM c'est obtenir en clair le secret de l'utilisateur NTLMv1, c'est NON.
### Ressource

The sources used for the creation of this article are the following :
- https://beta.hackndo.com/ntlm-relay/
- https://www.crowdstrike.com/cybersecurity-101/ntlm-windows-new-technology-lan-manager/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:00 | Page creation |