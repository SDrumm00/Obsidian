# How to Install Nvidia Drivers on [[Proxmox]]

#Proxmox #PVE #NVidia #Linux #Debian

1. Your `/etc/apt/sources.list` should look like this:
```bash
deb http://ftp.us.debian.org/debian bullseye main contrib non-free non-free-firmware

deb http://ftp.us.debian.org/debian bullseye-updates main contrib non-free non-free-firmware

# security updates
deb http://security.debian.org bullseye-security main contrib non-free-firmware

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

2. `apt update`
3. `apt upgrade`
4. `apt install pve-headers`
5. `apt install libnvidia-cfg1 nvidia-kernel-source nvidia-kernel-common nvidia-driver`
6. Reboot
7. `nvidia-smi` should print something like this:
```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.141.03   Driver Version: 470.141.03   CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA RTX A4000    On   | 00000000:42:00.0 Off |                  Off |
| 41%   44C    P8     7W / 140W |      1MiB / 16117MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```
