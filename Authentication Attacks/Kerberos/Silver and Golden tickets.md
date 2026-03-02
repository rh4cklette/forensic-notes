---
tags:
  - Unfinished
  - FR
  - MissingSources
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
This article is clearly a resume of the great https://beta.hackndo.com/kerberos-silver-golden-tickets/.

Si un attaquant parvient à compromettre le <b>hash NT</b> d'un <b>compte de service</b> et extraire le mot de passe ou le hash NT (via Kerberoasting par exemple) il peut alors <b>forger un ticket de service</b>, en choisissant ce qu'il va mettre dedans, sans passer par le KDC

Ce ticket est appelé Silver Ticket 

En gros :

<b>NT Hash compte de service</b> => <b>Silver ticket</b>, création de ticket de service sans passer par le KDC en choisissant les informations qu’il veut mettre dedans afin d’accéder à ce service.

<b>Hash</b> du <b>krbtgt</b> => <b>Golden ticket</b>, création de TGT sans passer par le KDC pour n'importe quel user du domaine => devient instant domaine admin.

<u>Long explanation silver tickets</u>

Prenons en exemple un attaquant qui trouve le hash NT du compte de la machine DESKTOP-01. Le compte machine est alors DESKTOP-01$. L’attaquant peut créer un bloc de données correspondant à un ticket comme celui trouvé dans KRB_TGS_REP, Il indiquera le nom du domaine, le nom du service demandé sous sa forme SPN (Service Principal Name), le nom d’un utilisateur (qu’il peut choisir arbitrairement), son PAC (qu’il peut également forger).

Une fois cette structure créée, il chiffre le bloc enc-part avec le hash NT découvert, puis il peut créer de toute pièce un KRB_AP_REQ. En effet, il lui suffit d’envoyer ce ticket au service cible, accompagné d’un authentifiant qu’il chiffre avec la clé de session qu’il a arbitrairement choisie dans le ticket de service. Le service sera en mesure de déchiffrer le ticket de service, extraire la clé de session, déchiffrer l’authentifiant et fournir le service à l’utilisateur puisque les informations forgée dans le PAC indiquent qu’il a le droit d’utiliser ce service.

Notons que le PAC est doublement signé. Une première signature via le secret du compte de service, et une deuxième via le secret du contrôleur de domaine. Cependant, lorsque le service reçoit ce ticket, il ne vérifie en général que la première signature. En effet, les comptes de services ayant le privilège SeTcbPrivilege, signifiant que ces comptes peuvent agir en tant que partie du système d’exploitation (par exemple le compte local SYSTEM), ne vérifient pas la signature du contrôleur de domaine.


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://beta.hackndo.com/kerberos-silver-golden-tickets/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:15 | Page creation |

