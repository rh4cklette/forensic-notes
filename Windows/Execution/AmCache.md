---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

## Introduction

AmCache.hve is a Windows system file that is created to store information related to program executions. The artifacts in this file can serve as a huge aid in an investigation, it records the processes recently run on the system. 

### Localisation

- ```%SystemRoot%\AppCompat\Programs\Amcache.hve```

### Data available in the artefact

Amcache stores the following data for the executed applications : 
- Execution Path
- First executed time
- Deleted time
- First installation
- SHA1 of file => usefull with VirusTotal


There are other artifacts in function of the filetype (more details here https://www.forensafe.com/blogs/amcache.html)

### <u>Tools </u>

We can exploit this artifact with Amcache Parser from Zimmerman : 

Documentation : https://github.com/EricZimmerman/AmcacheParser

Download : (https://ericzimmerman.github.io/#!index.md)

Other tools : 
    - RegRipper
    - Kape

### Exemple 

![[AmCache.png]]
<u><em>Figure 1 : Exemple AmCache parsing</em></u>

Moreover it is important to note that the timestamp that corresponds to the writing on disk is "File ID Last Write Timestamp"

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Ressource

The sources used for the creation of this article are the following :
- https://github.com/EricZimmerman/AmcacheParser

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2024-07-16 16:50 | Page creation |
