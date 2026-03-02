---
tags:
  - Unfinished
  - FR
---
### Initiation à la délégation Kerberos

La délégation Kerberos est un mécanisme qui permet aux services d'usurper l'identité des utilisateurs auprès d'autres services. Par exemple, un utilisateur peut accéder à une application frontale, et cette application peut, à son tour, accéder à une API back-end avec l'identité et les autorisations de l'utilisateur.

La délégation Kerberos se présente sous trois formes : la délégation sans contrainte, la délégation avec contrainte et la délégation avec contrainte basée sur les ressources (RBCD).

#### Délégation sans contrainte

La délégation sans contrainte exige que les utilisateurs envoient leur ticket d'attribution de ticket (TGT) au service frontal (serveur A). Le service frontal peut ensuite utiliser ce ticket pour usurper l'identité de l'utilisateur auprès de n'importe quel service, y compris le service dorsal (serveur B).

[![](https://d27a6xpc502mz5.cloudfront.net/wp-content/uploads/images-screenshots/image1.png)](https://d27a6xpc502mz5.cloudfront.net/wp-content/uploads/images-screenshots/image1.png)

#### Délégation restreinte

La délégation restreinte permet au service frontal (serveur A) d'obtenir des tickets de service Kerberos pour les utilisateurs auprès d'une liste prédéfinie de services spécifiés par leur nom de principal de service (SPN), tels que le service dorsal, le serveur B.

[![](https://d27a6xpc502mz5.cloudfront.net/wp-content/uploads/images-screenshots/image2.png)](https://d27a6xpc502mz5.cloudfront.net/wp-content/uploads/images-screenshots/image2.png)

Notez que la délégation contrainte permet au service d'usurper l'identité d'utilisateurs à partir de rien, qu'ils se soient authentifiés auprès du service ou non. Beaucoup pensent que cela dépend de la configuration de l'attribut **TrustedToAuthForDelegation**. Cependant, lorsqu'elle est associée à la délégation contrainte basée sur les ressources, cette limitation peut être contournée, comme je l'[explique dans ce billet de blog](https://shenaniganslabs.io/2019/01/28/Wagging-the-Dog.html#when-accounts-collude---trustedtoauthfordelegation-who).

#### Délégation restreinte basée sur les ressources

La RBCD est très similaire à la délégation restreinte, sauf que le sens de la contrainte est inversé. Elle spécifie qui est autorisé à déléguer à un service plutôt que qui est autorisé à déléguer au service. En d'autres termes, si le serveur A est autorisé à déléguer au serveur B dans le cadre de la délégation restreinte, la contrainte sera configurée dans un attribut du serveur A. Dans le cadre de la délégation restreinte, elle sera configurée dans un attribut du serveur B.

[![](https://d27a6xpc502mz5.cloudfront.net/wp-content/uploads/images-screenshots/image3.png)](https://d27a6xpc502mz5.cloudfront.net/wp-content/uploads/images-screenshots/image3.png)

Une autre différence importante entre la délégation restreinte et le RBCD est que la délégation restreinte spécifie le SPN du service cible. En revanche, RBCD spécifie le SID du service d'origine dans un descripteur de sécurité.

### Privilèges requis

La configuration de la délégation sans contrainte et de la délégation avec contrainte nécessite le privilège SeEnableDelegation, qui, par défaut, n'est accordé qu'aux administrateurs de domaine. Par conséquent, même si un utilisateur dispose d'un contrôle total (GenericAll) sur un compte AD, il ne pourra configurer aucun de ces types de délégation Kerberos s'il ne dispose pas également du privilège SeEnableDelegation. Contrairement à la délégation sans contrainte et à la délégation avec contrainte, RBCD requiert le droit de modifier l'attribut msDS-AllowedToActOnBehalfOtherIdentity, mais aucun privilège particulier.

Il convient de noter que les utilisateurs ont besoin de privilèges spéciaux pour modifier la configuration de la délégation restreinte, mais qu'aucun privilège spécial n'est nécessaire pour modifier les SPN. Par conséquent, il peut être intéressant d'aborder les scénarios avec le compromis de la délégation restreinte sous un angle différent, en manipulant l'attribut SPN plutôt que la configuration de la délégation.


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://www.semperis.com/fr/blog/spn-jacking-an-edge-case-in-writespn-abuse/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-11-20 09:43 | Page creation |

