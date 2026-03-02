---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Introduction

DFIR ORC is an open-source incident response tool developed by ANSSI.  
It enables fast and reliable collection of forensic artifacts from Windows systems, even in compromised environments.  
The tool is designed for live forensics while minimizing the impact on the target system.  
It relies on predefined profiles and automated scenarios to standardize evidence collection.  
DFIR ORC can also automatically interface with and execute other forensic tools to extend analysis capabilities.

# Usage

In I-Tracing the CERT is responsible of the tool and configured it to integrate multiple other tools. The documentation to use the tool, collect artifacts, and the switches used are available in the same folder.

Using DFIR Orc will give you a .zip as output, which will contains files like follows : 

![[Pasted image 20251217145721.png]]
<u><em>Figure 1 : Exemple of DFIR Orc collection</em></u>

However those .7z contain **anonymized** logs, as .data type.
To deanonymize those logs, dfir-orc-archive-rebuilder.py should be used (available in resources).

## Rebuilder installation

Even thought this repository contains a setup.py, the installation is a bit tricky as this tool is a bit old.
The install indicated "py7zr" package, however newer versions of py7zr do NOT support some functions used in the script, so one should install pyzr <= 0.17.3.

```
pip install pyzr==0.17.3
```

## Rebuilder usage

The rebuilder can be used with the following command line pattern : 

```
dfir-orc-archive-rebuilder.py /path/to/dfir-orc-collection.7z /path/to/analyze/machinexyz -c .\sample.toml
```

It will then create a full folder tree like in a normal Windows environment, with the artifacts collected.

## Parsing

Now that you have a usable collection, check [[0 - Kape]] to parse your artifacts and start your forensic analysis. 
Remember that if you collected artifacts with the ITR collection, you have to rebuild/parse those too.


<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- DFIR Orc Archive rebuilder : https://github.com/nahotjan/dfir-orc-archive-rebuilder/tree/main

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-12-17 14:22 | Page creation |