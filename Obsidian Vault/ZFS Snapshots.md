---
Created:
  - 20240723 8:45 PM
Subject: ZFS Snapshots
tags:
  - ZFS
---
------------------
Note Links:
- [[ZFS Filesystem]]
- [[Sanoid]]
-------------------

# Abstract
I wish to be able to create snapshots of my file system and send them to a remote system for backup purposes.

I also wish to be able to rollback any unwanted file changes by being able to restore from a snapshot.
## Decision Point
1) Do I want to create my own scripts using shell or bash and then systemd unit-timers?
2) Do I want to use an existing tool like [[Sanoid]]

### Answer: 
[[Sanoid]]

## Discuss why not the other way


