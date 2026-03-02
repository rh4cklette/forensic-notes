---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

Create a body file and set the root of body file (in you are in /, set /, if you make a body file from a specific location, set it)

`fls -r -m / image.dd > body.txt`

You can use -l (small L) format for "long" format, indicating amongst other things : 
- mtime (last modified time)
- atime (last accessed time)
- ctime (last changed time)
- crtime (created time)
- birth (creation)

Once that body file is created, create the timeline : 

`mactime -b body.txt -z UTC <startingDate -p -q > timeline.txt`

If you want a timeline of all events, don't set `<startingDate>`

On a Unix system, the User and Group IDs can be mapped to actual names by using the '-p' and '-q' flags. The '-z' flag can be used to specify the time zone, if it is different from the local timezone.

# Side notes

Regarding timeline MACB format, you can refer to the following for more information :

![[68747470733a2f2f79617073382e6769746875622e696f2f6f735f74696d657374616d70732f323032322d30332d30332f756e69785f6d6163622e706e67.png]]

From : https://github.com/QuoSecGmbH/os_timestamps

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- Timeline MACB information : https://github.com/QuoSecGmbH/os_timestamps
- TSK documentation : https://wiki.sleuthkit.org/index.php?title=Mactime
- MACB Timestamp : https://github.com/QuoSecGmbH/os_timestamps

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-29 15:37 | Page creation |