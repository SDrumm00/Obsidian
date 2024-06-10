---
Created:
  - 20240311 3:46 PM
aliases: 
tags:
  - Emacs
  - System-Crafters
  - linux
  - Building-A-Second-Brain
  - GNU
  - Org-Mode
  - Org-Roam
  - IDE
  - developer
Subject: GNU Emacs Learning
---
----------------
# What is Doom Emacs?
https://github.com/doomemacs
"A configuration framework for GNU Emacs"

> It's a tale as old as time: a stubborn, shell-dwelling, and melodramatic vimmer -- tormented by Vimscript and his boundless productivity -- makes a formal request to the netherworld for a transfer. They agree. The terms? He must lure more unsuspecting souls into a life of eternal bikeshedding. Now he runs the place.

Doom Emacs is a configuration framework for [GNU Emacs](https://www.gnu.org/software/emacs/) with an optional starter kit attached. It's tailored for [bankruptcy veterans](https://www.emacswiki.org/emacs/DotEmacsBankruptcy) seeking a faster, more reliable, and reproducible foundation for their next config, or for beginners who want a softer introduction to our favorite operating system.

## Getting Started
On Debian Linux, run the below commands.
### Quick Installation
```
git clone --depth 1 --single-branch https://github.com/doomemacs/doomemacs ~/.config/emacs
~/.config/emacs/bin/doom install
```

There will be a prompt within the terminal
```
Generate an envvar file? (see `doom help env` for details) (y or n) 
```
Just hit yes. If you want to know more about this then there is a reference to "see doom env help" for details. Which is in the documentation on the git site listed above.


