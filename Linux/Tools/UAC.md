---
tags:
  - EN
  - Unfinished
---
<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>
# Introduction

To paraphrase the documentation : 
`UAC (Unix-like Artifacts Collector) is a powerful and extensible incident response tool designed for forensic investigators, security analysts, and IT professionals. It automates the collection of artifacts from a wide range of Unix-like systems, including AIX, ESXi, FreeBSD, Linux, macOS, NetBSD, NetScaler, OpenBSD and Solaris.

# Usage

## Customization : 

List the available profiles with : `uac -p list`
To confirm a custom profile is valid use : `uac --valide-profile <profilePath.yaml>`

Profiles are defined in profile directory and formated in yaml file.
Those profile contain either directly the tools to use for the collection: 

![[Pasted image 20260205114813.png|300]]

Configuration files define how to collect the artifacts : 
![[Pasted image 20260205115030.png]]

For more information on the creation / analysis of those files, see : https://tclahr.github.io/uac-docs/artifacts/#collectors-overview

# Usage

To collect the artifacts using the default full collection use : 

![[Pasted image 20260205140414.png]]

For remote Transfer arguments see : https://tclahr.github.io/uac-docs/#-sftp

Once the output is uncompressed this should output the following folders : 
![[Pasted image 20260205141510.png]]

- The bodyfile can directly be ingested by [[TSK]]
- The hash_executables can be passed to `debsums`to verify packet integrity

<div style="text-align: center;"><font color="#FFFFFF" style="background-color:#000000;" size=+3> TLP:CLEAR </font></div>

### Resources

The sources used for the creation of this article are the following :
- https://tclahr.github.io/uac-docs

### Page creation

**Page creator:** rhacklette
| Date | Trigram | Action |
| --- | --- | --- |
| 2026-01-26 14:10 | Page creation |