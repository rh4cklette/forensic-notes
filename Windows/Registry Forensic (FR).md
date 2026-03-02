---
tags:
  - FR
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

This page is the result of a discussion with a friend, his comments are left as is. Big UP GBE, you'll recognize yourself.

# Autostart location

Run key : 
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run (available BOOKMARKS Registry Explorer)`
- `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce (available BOOKMARKS Registry Explorer)`
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run (available BOOKMARKS Registry Explorer)` 
- `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce (available BOOKMARKS Registry Explorer)`

Les Run/RunOnce de Software sont communes à tous les users, les autres sont spécifiques à chaque user

#### Userinit key

- `HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersionWinlogon`

Default value: C:\Windows\system32\userinit.exe (queried by winlogon) Si c'est quelque chose d'autre qu'userinit.exe, il faut investiguer.

##### Running Services

• `HKLM\SYSTEM\CurrentControlSet\Services\* (available BOOKMARKS Registry Explorer)`

Je me concentre en général sur les "Automatic" services qui se lancent au reboot et les services qui matchent parfaitement la date et l'heure de l'incident (sinon trop de service legitime à analyser)

## Network activity

<u>Known networks</u>

- `HKLM\System\Microsoft\Windows NT\CurrentVersion\NetworkList (available BOOKMARKS Registry Explorer)`

Le bookmarks regroupent tous les known networks avec le first connect / last connect

<u>Machine IP</u>

• `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces`

2 bookmarks disponibles : DHCPNetworkHints et NetworkSettings

<u>Outgoing RDP sessions</u>

• `HKCU\Software\Microsoft\Terminal Server Client`

Si il y a une valeur dans le bookmarks "TerminalServerClient" alors on sait que l'utilisateur en question s'est connecté en RDP sur la machine citée dans la clé de registre. Le timestamp correspond au début ou la fin de la connexion (il faut revérifier mais j'ai pas de case avec le cas de figure pour tester)

## Suspicious users

<u>TypePath/TypedUR</u>

- `HKCU\Software\Microsoft\Internet Explorer\TypedURLs (available BOOKMARKS Registry Explorer)`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths (available BOOKMARKS Registry Explorer)`

## URL and Windows Explorer path

<u>RecentDocs</u>

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs (available BOOKMARKS Registry Explorer)`

=> Docs accessed by extensions

<u>UserAssist</u>

- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist (available BOOKMARKS Registry Explorer)`

Access items => executed programs

- `HKLM\SAM\Domains\Account\Users (available BOOKMARKS Registry Explorer)`

SAM (je ne le check pas à chaque fois mais quand j'ai un compte suspect)

Creation time, last login, last password change, last incorrect password des utilisateurs locaux

## Program Execution

<u>AppCompatCache</u>

- `HKLM\System\ControlSet001\Control\Session Manager\AppCompatCache`

Le modified time n'est pas fiable, ce n'est pas forcément la date d'exécution mais ça prouve la présence du programme à un moment donné sur l'asset

## Exfiltration

<u>7zip</u>

- `HKCU\Software\7-Zip\Compression\ArcHistory`
- `HKCU\Software\7-Zip\Extraction\PathHistory`
- `HKCU\Software\7-Zip\FM /v "CopyHistory"`
- `HKCU\Software\7-Zip\FM /v "FolderHistory"`

Quand je cherche de l'exfiltration je check toujours ces clés-là par précaution mais j'ai rarement eu des résultats (une fois il me semble)

<u>USB</u>

- `HKLM\System\MountedDevices`
- `HKLM\System\CurrentControlSet\Enum\USBSTOR`
- `HKLM\System\CurrentControlSet\Enum\USB`
- `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2`

Il faut combiner toutes ces clés pour avoir les infos sur qui a utilisé tel device (type, fabriquant, etc.) et quand (first and last time) - cf. le poster du SANS pour plus de détail, je ne connais pas par cœur, je revérifie à chaque fois.

## Divers

<u>Computer Name</u>

- `HKLM\System\ControlSet001\Control\ComputerName\ComputerName`

<u>Domain Name</u>

- `HKLM\Security\Policy\PolDnDDN`

Il faut convertir l'hexadecimal, registry explorer le fait pour nous

<u>App Path</u>

- `HKLM\Software\Microsoft\Windows\CurrentVersion\App Paths`

Le bookmark regroupe tous les exe avec leur location, intéressant quand tu as malware mais pas son path.

Or system-wide applications: File: 
- `%SystemRoot%\System32\config\SOFTWARE` 
- Registry key: `Microsoft\Windows\CurrentVersion\App Paths` 
- For per-user applications: File: `%SystemDrive%:\Users\<USERNAME>\NTUSER.dat` Registry key: `HKCU\Software\Microsoft\Windows\CurrentVersion\App Paths`

Je ne sais pas à quoi correspond le timestamp dans cette clé

## ProfileList

- `HKLM\Software\Microsoft\Windows NT\CurrentVersion\ProfileList`

Le bookmark regroupe l'ensemble des SID avec l'image path associé

`HKEY_CURRENT_USER\Software\Classes` => With the user association for file type
`HKEY_LOCAL_MACHINE\Software\Classes` => with the system's associations for file type


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-02-02 14:45 | Page creation |
