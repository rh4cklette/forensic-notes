---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

To identify malicious plugins, one can use the following script. For plugin list extraction see [[Database analysis]] : 

```
import re
import requests
import urllib3
import time
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)


# Données d'entrée : liste sérialisée PHP
# Extracted from SELECT option_value FROM <db_name>_options WHERE option_name='active_plugins';
serialized_plugins = """
i:9;s:22:"hello-dolly1/hello.php";
i:25;s:24:"wpforms-lite/wpforms.php";
"""

# Extraction du slug du plugin (dossier avant le /)
plugin_slugs = set(re.findall(r'"([^/]+)/[^"]+\.php"', serialized_plugins))

base_url = "https://wordpress.org/plugins/"

print("[*] Vérification des plugins WordPress\n")

for slug in sorted(plugin_slugs):
    time.sleep(5)
    url = f"{base_url}{slug}/"
    try:
        response = requests.get(url, timeout=10, allow_redirects=True, verify=False)
        print(f"{response.url} for {slug}")
        if response.status_code != 200 or '/search/' in response.url :
            print(f"[!] NON OK ({response.status_code}) : {slug}")
    except requests.RequestException as e:
        print(f"[!] ERREUR REQUÊTE : {slug} ({e})")

print("\n[*] Analyse terminée")

```

The trick is based on the fact that if a plugin is official, it will have an URL pointing to it. If not, wordpress redirects to "search" to search the plugin.

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- Own experience. 

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-21 16:31 | Page creation |