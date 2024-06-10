---
Created:
  - 20240405 3:51 PM
aliases:
  - DWM
  - Suckless
  - Linux
  - Debian
tags:
  - Debian
  - linux
Subject: Debian Suckless
Note Links: "[[Debian Suckless|Linux]]"
---
--------
# Abstract

Instructions for setting up [[Debian]] with the [[Suckless]] software ecosystem (eg. [[DWM]], [[DMenu]])

Environment:
Virtual Environment - Virt-Manager VM
Debian 12
Kernel 6.1


Steps:
1) Install Debian in vm
	1) Use the netinstall
2) Arrive at the TTY
3) Run: 
	1) `sudo apt update && sudo apt upgrade`
4) Install xserver-xorg-core xinit xinput x11-xserver-utils
5) Install git wget build-essential make kitty feh pipewire firefox-esr lightdm picom ranger
6) Install libx11-dev libxinerama-dev libxft-dev
7) mkdir ~/.config
8) Run:
	1)  `git clone https://git.suckless.org/dwm`
9) cd into the dwm directory
10) Run:
	1) `nano config.def.h`
11) Change the terminal to Kitty
	1) search for the line
		`static const char *termcmd[] = { "st", NULL }:`
	2) Change it to
		`static const char *termcmd[] = { "kitty", NULL }:`
	3) Save and exit
11)   Copy the config.def.h file to config.h
	1) `cp config.def.h config.h`
12) make
13) sudo make install
14) cd ..
15) git clone httpos://git.suckless.org/dmenu
16) cd dmenu
17) sudo make
18) sudo make install
19) cd ~
20) setup a `~/.xinitrc` with at least `exec dwm`
	1) Aside: use XRandR for setting the screen resolution here as well by adding the following line to the .xinitrc file
		`xrandr --output <name of display> --mode <resolution size>`
		2) All together, the 2 things are added one after the other with a semi-colon between them like the following
		`xrandr --output <name of display> --mode <resolution size>; exec dwm`
21) run: 
	1) `startx`
22) Set the background wallpaper
	1) Pick an image and place it in e perma directory
	2) update the .xinitrc file with the following
	   `feh --bg-fill <path to image>;`
	3) ensure that it sets the wallpaper after the resolution and before dwm itself is executed 

When adding a Login Manager
(LightDM in this case)

Read this.
https://blkct.wordpress.com/2017/06/16/how-to-start-dwm-from-lightdm/

# [HOW TO] Start dwm from LightDM

1. Create dwm menu entry for window manager.

 $ sudo nano /usr/share/xsessions/dwm.deskto

Desktop Entry]
Encoding=UTF-8
Name=Dynamic Window Manager
Comment=Runs the window manager defined by xsession script
Exec=/etc/X11/Xsession

2. Then, create xsession script.

$ nano ~/.xsession

#!/bin/bash

# source for the terminal
xrdb -merge $HOME/.xres &

# font
xset fp+ $HOME/.fonts &
xset fp rehash &

# set keyboard layout to us
setxkbmap us &

# Start your wm!
exec dwm

TODO:
- Now to resolve the background wallpaper settings getting lost. Must be in a different config file => they were in the .xsession config file
- The resolution is wrong again, so with that new .xsession file, we must be ignoring the .xinitrc file now => confirmed

## Picom
install it
load the config file into the ~/.config/picom directory
make sure to autostart it in the .xsession config file
that cmd for the .xsession file looks like the following
`picom --config /home/scott/.config/picom/picom.conf`
