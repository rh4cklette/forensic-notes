---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Reminders

The Security Account Manager (SAM) is a particular registry hive that stores credentials and account information for local users

SAM Database (``%SystemRoot%\System32\config\SAM``) => Local accounts

Starting with Windows 2000 and above, the SAM hive is also encrypted by the SysKey by default in an attempt from Microsoft to make the hashes harder to access. However, the SysKey can be extracted from the SYSTEM registry hive, which can be located at ``%SystemRoot%\System32\config\SYSTEM

Adversaries can calculate the Syskey by using RegOpenKeyEx/RegQueryInfoKey API calls to query the appropriate class info and values from the 
- ``HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\JD,
- HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\Skew1,
- HKLM:\SYSTEM\CurrentControlSet\Control \Lsa\GBG,
- ``HKLM:\SYSTEM\CurrentControlSet\Control\Lsa\Data keys.

Further resources on forensic and how to detect credential dumping can be found here : 
- https://www.praetorian.com/blog/how-to-detect-and-dump-credentials-from-the-windowsregistry/

# LSA Secrets

LSA secrets is a special protected storage for important data used by the Local Security Authority (LSA) in Windows. LSA is designed for managing a system's local security policy, auditing, authenticating, logging users on to the system, storing private data. Users' and system's sensitive data is stored in secrets. Access to all secret data is available to system only.

What kind of data is stored in these secrets? Here are a few that I've come across:

- $MACHINE.ACC: has to do with domain authentication, see KB175468
- DefaultPassword: password used to logon to Windows if auto-logon is enabled
- NL$KM: secret key used to encrypt cached domain passwords
- L$RTMTIMEBOMB_[...]: FILETIME giving the date when an unactivated copy of Windows will stop working

=> Domain accounts can be found as well as service accounts
=> If the service account is part of domain admins, the attacker gets a domain admin account


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
-  https://threathunterplaybook.com/notebooks/windows/07_discovery/WIN-190625024610.html
- https://www.ired.team/offensive-security/credential-access-and-credential-dumping/dumping-andcracking-mscash-cached-domain-credentials
- <http://moyix.blogspot.com/2008/02/decrypting-lsa-secrets.html>
- https://devblogs.microsoft.com/scripting/use-powershell-to-decrypt-lsa-secrets-from-the-registry/
- https://www.praetorian.com/blog/how-to-detect-and-dump-credentials-from-the-windowsregistry/
- Crack domain credentials : https://www.ired.team/offensive-security/credential-access-and-credential-dumping/dumping-andcracking-mscash-cached-domain-credentials

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-17 11:36 | Page creation |