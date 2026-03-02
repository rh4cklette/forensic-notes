---
tags:
  - Unfinished
  - FR
  - Categories
---
%% Begin Waypoint %%
- [[AS-REP Roasting]]
- [[AS-REQ Roast]]
- **[[DACL-ACE attacks]]**
	- [[Abusing RBCD]]
	- [[Shadow Credentials]]
	- [[SPN Jacking]]
- [[Pass the key OR Overpass the hash]]
- [[Pass-the-ticket]]
- [[Silver and Golden tickets]]
- [[UNPac the hash]]

%% End Waypoint %%
Intro et fonctionnement :

Protocole d'authentification qui prends la suite de NTLM. C'est le protocole d'authentification le plus utilisé.

Il se fait en 6 étapes :

![[Pasted image 20240715172705.png]]

Ou de façon plus détaillée (sera utile pour comprendre les attaques exploitant le protocole d'authentification)

![[Pasted image 20240715172720.png]]

And with PAC validation : 

![[Pasted image 20240715172736.png]]

De manière générale, il faut comprendre que les TGT est fournis par le KDC et le client utilise son mot de passe pour déchiffrer le TGT, ce qui lui permet de demander des TGS qui permettent de s'authentifier auprès du service auquel le client veut accéder.

Ici quand on parle de TGS on ne parle pas du Ticket Granting Server mais bien du Ticket Granting Service, utilisé pour accéder à un service.

Détails :

Windows fait intervenir un autre mécanisme, en quelque sorte une extension du protocole Kerberos, appelé le PAC, qui permet la bonne gestion des droits dans un AD. Ce PAC est présent dans les TGT et dans les TGS.

AS_REQ : transmit en clair contient : l'identité de l'user, le nom du service auquel il veut accéder, la durée de vie du ticket ainsi que la liste des machines où il peut être utilisé. Inclut des données de pré-authentification, dont un timestamp chiffré avec le mot de passe du user

AS_REP : retour une TGT (Ticket-Granting Ticket), ce ticket est chiffré avec la clé secrète du TicketGranting Server ainsi qu'une clé de session (SKTgs) chiffrée avec la clé secrète de l'utilisateur. Le TGT est chiffré avec le compte krbtgt.

TGS_REQ : Demande de ticket d'authentification après d'un service, aka TGS. Contient le TGT, et username + timestamp chiffré avec la clé de session SKTgs.

TGS_REP : A la reception du TGS_REQ le Ticket-Granting Server déchiffre le TGT et vérifie sa validité, si le TGS_REQ est valide, le Ticket Granting Server emet un TGS pour le client. + clé de session SKService

La réponse KRB_TGS_REP est composée de deux parties. Une première partie est le ticket de service dont le contenu est chiffré avec le secret du service demandé, et une deuxième partie est une clé de session qui sera utilisée entre l’utilisateur et le service. Le tout est chiffré avec le secret de l’utilisateur.

AP_REQ : Vérifie que le username présent dans l'authentificateur est == celui dans le ticket de service.

Dans le cas d'utilisation d'un service sur un équipement tiers, une autre requête (AP_REQ) est envoyée au serveur fournissant le service.

Cette requête contient :

- un nouvel authentificateur, chiffré cette fois avec SKservice ;
- le ticket de service TS.

Le fournisseur de service commence par extraire la clé SKservice présente dans le ticket de service TS. À l'aide de cette clé, il extrait le nom d'utilisateur présent dans l'authentificateur et s'assure qu'il est identique à celui contenu dans le ticket de service.

AP_REP : Permet de s'assurer une authentification mutuelle entre client et service.

Ce dernier échange, qui est optionnel, est envoyé uniquement afin d'assurer une authentification mutuelle entre le client et le serveur fournissant le service (V).

Cette requête contient un timestamp généré par le serveur et est chiffrée avec la clé SKservice
### Ressource

The sources used for the creation of this article are the following :
- https://connect.ed-diamond.com/MISC/misc-054/attaque-sur-le-protocole-kerberos
- https://www.manageengine.com/products/active-directory-audit/kb/images/event-4771-kerberosauthentication-illustration.jpg
- https://software.intel.com/sites/manageability/AMT_Implementation_and_Reference_Guide/ImagesExt/image861_0.png
- https://connect.ed-diamond.com/MISC/misc-110/pre-authentification-kerberos-de-la-decouverte-al-exploitation-offensive
- https://beta.hackndo.com/kerberos-silver-golden-tickets/#pac
- https://unit42.paloaltonetworks.com/next-gen-kerberos-attacks/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:26 | Page creation |