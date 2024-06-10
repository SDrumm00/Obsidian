---
Created:
  - 20240312 9:07 AM
aliases: 
tags:
  - woorkstation
  - hardware
  - linux
Subject: Workstation Configuration
---
-------------------------
## Current Configuration
This article is primarily to discuss the currect state of my workstation in relation to it's [[Hardware/Workstation/Workstation Roadmap|Workstation Roadmap]] and to see how we are progressing towards that goal.
### Operating System
I am using [[Debian]] [[Linux]] as my OS of choice at the time of this writing.
### Filesystem
[[ZFS]]
I use [ZFS On Linux](https://zfsonlinux.org/) as my main filesystem. It's interesting really, I am good enough to put my /home directory on a zpool and keep it even when I wipe my root disk and start clean and I then simply re-mount it... But not good enough to Root of ZFS which requires additional setup....

So at this time, I am using EXT4 as my root disk filesystem.

I have enabled some cerature features of ZFS to make it better.
- L2ARC read cache disks
- SLOG write cache disks
- Enables AutoTrim