---
tags:
  - EN
  - JustStarted
---
<div style="text-align: center;"><font color="#FF2B2B" style="background-color:#000000;" size=+3> TLP RED </font></div>
# Introduction

The `$MFT` (Master File Table) is a critical NTFS system file that records metadata for every file and directory on a volume.  
In forensic investigations, it helps reconstruct file timelines, including creation, modification, and access times.  
It can reveal deleted files and their original locations, even if the data is no longer accessible.  
Investigators use `$MFT` to track file movements, renames, and suspicious activity patterns.  
Analyzing it provides a detailed overview of filesystem activity crucial for understanding a compromise.

# Tool

To collect $MFT use either :
- Kape
- DFIR Orc

To parse the output use either Timeline Explorer or ingest it in your favorite SIEM.

See the Tools section for more information

<div style="text-align: center;"><font color="#FF2B2B" style="background-color:#000000;" size=+3> TLP RED </font></div>

### Resources

The sources used for the creation of this article are the following :
- Source ?

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-12-16 16:32 | Page creation |