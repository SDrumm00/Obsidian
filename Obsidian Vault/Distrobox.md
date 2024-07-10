---
Created:
  - 20240705 8:43 AM
Subject: Distrobox
tags:
---
---------------
Note Links:
- [[Linux]]
- [[Podman]]
- [[Docker]]
---------------------
# About
https://github.com/89luca89/distrobox

Use any linux distribution inside your terminal. Enable both backward and forward compatibility with software and freedom to use whatever distribution you’re more comfortable with.

On Debian, I plan to use Podman as it fits nicely with the Podman AI labs I wish to use for local LLM's.
 See https://www.reddit.com/r/LocalLLaMA/ for reviews on best local LLM's outside of the Podman solution.
 

## Getting started
Installation on Debian 12 is easy with 
`apt install distrobox`

By default, it will install podman as a dependency so easy peezy.
At the time of installation, the version installed was:
```
distrobox version
distrobox: 1.4.2.1
```

### Install Lutris on an image of Arch
1) `distrobox create --name Lutris --image fedora:latest`
2) `distrobox enter Lutris`

Once inside the pod named Lutris, simply install Lutris
`sudo pacman -S Lutris`

It will install the necessary things. Now to test it by running the command
`lutris`

Error messages:
scott@Lutris:~$ lutris
2024-07-08 18:20:08,333: Command 'vulkaninfo' not found on your system
2024-07-08 18:20:08,334: Command 'wine' not found on your system
2024-07-08 18:20:08,334: Command 'fluidsynth' not found on your system
2024-07-08 18:20:08,634: Couldn't find a terminal emulator.
2024-07-08 18:20:08,744: The Battle.net source is unavailable because Google protobuf could not be loaded: No module named 'google'
Gtk-Message: 18:20:08.784: Failed to load module "canberra-gtk-module"

Resolution:
1) Ensure wine-staging is installed
	1) Requires multilib library enabled in the pacman.conf file

execute `lutris`
2024-07-10 09:52:48,217: Command 'vulkaninfo' not found on your system
2024-07-10 09:52:48,218: Command 'fluidsynth' not found on your system
2024-07-10 09:52:49,015: The Battle.net source is unavailable because Google protobuf could not be loaded: No module named 'google'
Gtk-Message: 09:52:49.054: Failed to load module "canberra-gtk-module"

Progress made!
Ok next is vulkaninfo
- install [nvidia-utils](https://archlinux.org/packages/?name=nvidia-utils) (or [lib32-nvidia-utils](https://archlinux.org/packages/?name=lib32-nvidia-utils)) - NVIDIA proprietary
- requires a distrobox restart

Fluidsynth
`pacman -S fluidsynth`

canberra-gtk-module error
`pacman -S libcanberra`




Once complete, export the Lutris app to your host desktop


