---
tags:
  - EN
  - Finished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

# Introduction

Kape is a tool regrouping Eric Zimmerman's Tools. This guy is an absolute legend of forensics and his tools WILL help you in investigation.

The list of all his tools can be found in the resources section and they'll help you for all the artifacts defined on this wiki. However, each tool treats one artifact, and during an investigation when you need to treat multiple artifacts and parse them, those are a bit slow.

Kape / GKape comes to the rescue, it allows to parse multiple artifacts and use modules on them to convert the output as CSV, that you can later use in Timeline Explorer from Zimmerman.

For the Target options, appart from specific needs I recommend to use:
- !SANS_Triage

For Module options, appart from specific needs, use : 
- !!ToolSync
- !EzParser

If the collect needs to be done by the client, you can always configure all the options in the GUI, and send Kape and the associated command line to the client.

![[Pasted image 20251217113648.png|600]]
<u><em>Figure 1 : GKape interface with command line visible at the bottom </em></u>
# Usage with TimelineExplorer

After modules finish, you'll be left with .csv files for each artifacts you parsed.
These CSV can be ingested in your favorite SIEM for analysis, or used in Timeline Explorer from Zimmerman. You can then filter each column individualy, change the column orders etc : 

![[Pasted image 20251217114817.png]]
<u><em>Figure 2 : Filtering in Timeline Explorer</em></u>

# Going Further

If you are curious enough, and want to know what each Target options are, double click the name of the option.
It indicates the Path of the source file defining each Target.
For exemple, for target WSL : 

![[Pasted image 20251216170927.png|200]]

This means that if you go to KAPE\Targets\Windows\WSL, you'll have all tkape files, and within all the definitions related to the targets : 

![[Pasted image 20251217113247.png]]

And the documentation/article used to create the target : 

![[Pasted image 20251217113329.png|500]]

Use that to get good information on what you are collecting and expand your knowledge of forensics. Tools are good, but you need to understand the artifacts you are dealing with.

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
### Resources

The sources used for the creation of this article are the following :
- Kape : https://www.kroll.com/en/services/cyber/incident-response-recovery/kroll-artifact-parser-and-extractor-kape
- Zimmerman Tools : https://ericzimmerman.github.io/#!index.md

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2025-12-16 16:17 | Page creation |