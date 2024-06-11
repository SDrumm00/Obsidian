---
Created:
  - 20240611 11:53 AM
aliases:
  - Suckless
tags:
  - Debian
  - linux
  - Suckless
  - Dmenu
Subject: Dmenu Tips and Tricks
Note Links:
---
------------------
# Abstract
Things to know about DMenu

## Flatpaks
They do not show in the call up because the apps are not installed in the traditional Linux /usr/bin directory.

Therefore, one must create a sym-link to the binary in order to see it in [[DMenu]]

Example of how to create a symlink for Lutris flatpaks.

`sudo ln -s /var/lib/flatpak/exports/bin/net.lutris.Lutris /usr/bin/lutris`
