---
tags:
  - Unfinished
  - FR
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
### Key Takeaways


![[Pasted image 20240715172001.png]]

![[Pasted image 20240715172017.png]]

#### Details 

Technique utilisée pour obtenir un TGT en ayant uniquement connaissance du NT-Hash (ou NTLM hash)d'un user. NT-Hash généralement obtenu via dump LSASS + mimikatz.

Utile dans les réseaux où le protocole NTLM est disabled, et seulement Kerberos est allowed

Il faut que le chiffrement du TGT soit RC4. pour pouvoir avoir un NT-Hash en mémoire

Kerberos offers 4 different key types: DES, RC4, AES-128 and AES-256.

- RC4 type is enabled, the RC4 key can be used. The problem is that the RC4 key is in fact the user's NT hash. Using a an NT hash => TGT => overpass the hash.
- When RC4 is disabled, other Kerberos keys (DES, AES-128, AES-256) can be passed as well => Pass the Key => only the name and key used differ between overpass the hash and pass the key => technique is the same.

(En gros au lieu d'utiliser le hash NTLM, il n'est possible d'utiliser QUE les CLES (AES128,AES256) par exemple)
### Ressource

The sources used for the creation of this article are the following :
- https://connect.ed-diamond.com/MISC/misc-110/pre-authentification-kerberos-de-la-decouverte-a-l-exploitation-offensive
- https://www.thehacker.recipes/ad/movement/kerberos/ptk
- https://blog.gentilkiwi.com/securite/mimikatz/overpass-the-hash

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-15 17:19 | Page creation |