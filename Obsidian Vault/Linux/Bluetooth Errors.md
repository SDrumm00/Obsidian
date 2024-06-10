---
Created:
  - 20240405 1:36 PM
  - 20240405 2:34 PM
aliases: 
tags:
  - Bluetooth
  - linux
  - Errors
Subject: Bluetooth Errors
Note Links: "[[Bluetooth]]"
---
--------------------------------------
# Abstract

Here are the list of errors I have seen in my logs and how to fix them on various linux hosts.

## src/plugin.c:plugin_init() Failed to init vcp plugin
## src/plugin.c:plugin_init() Failed to init mcp plugin
## src/plugin.c:plugin_init() Failed to init bap plugin

- Debian 12 Linux
- 2024
- cite: https://forum.manjaro.org/t/bluetoothd-errors-how-to-fix-them/134588
	- cite: https://wiki.archlinux.org/title/Bluetooth#Enabling_experimental_features

To repeat and sum up:  
There were two groups of errors, and the first one was a simple notification that some experimental options are off. Turning experimental options on, made the notifications go away.  
The second group is generated after the list of Bluetooth devices that are remembered and system is trying to connect with them automatically. Since those aren’t available all the time, they generate a connection error in journal.

All is clear. Thanks again!

Solution

