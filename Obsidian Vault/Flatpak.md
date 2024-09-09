---
Created:
  - 20240903 9:18 AM
Subject: Flatpak
tags:
  - Flatpak
  - Linux
Note Links: "[[Flatpak]]"
Back Links: "[[Linux]]"
---
----------------
# Abstract
Flatpak plus symlinks to get them to appear in DMenu

## Instructions
To run a flatpak app from dmenu, you can create a symlink for the app in `/usr/bin`. You can find the flatpak apps binary link in `/var/lib/flatpak/exports/bin/` on ubuntu. E.g. to run Rocket Chat (installed from flatpak via software center), you can do something like this:

```
sudo ln -s /var/lib/flatpak/exports/bin/chat.rocket.RocketChat /usr/bin/rocket-chat
```
