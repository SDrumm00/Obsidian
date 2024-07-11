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

## GUI
Once the core application is installed, take a look at [BoxBuddy](https://github.com/Dvlv/BoxBuddyRS)
It can also be installed via Flatpak [HERE](https://flathub.org/apps/io.github.dvlv.boxbuddyrs)
