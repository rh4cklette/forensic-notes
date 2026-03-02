---
tags:
  - EN
  - Finished
  - Persistance
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

## Introduction

COM model defines a model allowing inter process communication and code reuse. Com Hijacking allows to execute code from a legitimate process and can allow to establish persistance.

For example :

![[ComObjectForensic_1.png]]

(https://blog.maltrak.com/com-objects-p-1-the-hidden-backdoor-in-your-system-947ac4285e85)

<u>More information here :</u>

https://pentestlab.blog/2020/05/20/persistence-com-hijacking/


Registry key linked to COM Objects and their manipulation is in the following form : 

``HKEY_CLASSES_ROOT\CLSID\{…}

However, because the HKEY_CLASSES_ROOT hive is actually combined data found in both 
HKEY_LOCAL_MACHINE hive (
```HKEY_LOCAL_MACHINE\Software\Classes```) and the
HKEY_CURRENT_USER hive (```HKEY_CURRENT_USER\Software\Classes```), it also contains user-specific data.

<u>Reminder :</u>

HKEY_CURRENT_USER => Hive of the actual user => NTUser.dat to gather and analyze.

<u>How does it work ?</u>

Programs that search for COM objects defined by CLSID that do not exist can be exploited.
When the keys do not exist, an attacker can create manualy a key in the correct registers, and make them point to a malicious local file (in the case of keys "InprocServer32" and "LocalServer32") or malicious remote file with the key "ScriptletURL".

<u>Which tool ?</u>

- Registry explorer from Zimmerman : https://www.sans.org/tools/registry-explorer/
- EDR and monitoring tools

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://blog.maltrak.com/com-objects-p-1-the-hidden-backdoor-in-your-system-947ac4285e85
- https://pentestlab.blog/2020/05/20/persistence-com-hijacking/

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 15:22 | Page creation |
