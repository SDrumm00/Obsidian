#zfs #linux #learning #btrfs #virtualization

ZFS on Root on [[Debian]]
https://www.youtube.com/watch?v=7F7Ch-ZkiQU

I generally do not recommend ZFS on Root for [[Linux 1]]. The better way of doing this is by using EXT4 or the default file system for the distro and then using a zpool to store your /home directory on.

At the time of this writing (20231030) I a, using Ubuntu Server with a mirrored root file system and a zpool file system for my /home directory and everything is very robust and and resilient.

[[ZFS Filesystem 1]] vs [[BTRFS]]
- ZFS wins
 
[[2023 Learning Goals]] aligns to my learning more about Linux

ZFS commands
- zpool status - shows status of the pool specified
- zpool clear - removes errors of the specified zpool
- zpool remove - removes any specified devices within the zpool

## Virt-Manager Virtualiztion
For some odd reason, I cannot get the QEMU Virt-Manager application to create storage on a [[ZFS Filesystem 1]] data set. I get an odd error stating it does not exist.

### Troubleshooting
Unable to complete install: 'unsupported configuration: storage type 'dir' requires use of storage format 'fat''

Traceback (most recent call last):
  File "/usr/share/virt-manager/virtManager/asyncjob.py", line 72, in cb_wrapper
    callback(asyncjob, *args, **kwargs)
  File "/usr/share/virt-manager/virtManager/createvm.py", line 2008, in _do_async_install
    installer.start_install(guest, meter=meter)
  File "/usr/share/virt-manager/virtinst/install/installer.py", line 695, in start_install
    domain = self._create_guest(
  File "/usr/share/virt-manager/virtinst/install/installer.py", line 637, in _create_guest
    domain = self.conn.createXML(initial_xml or final_xml, 0)
  File "/usr/lib/python3/dist-packages/libvirt.py", line 4400, in createXML
    raise libvirtError('virDomainCreateXML() failed')
libvirt.libvirtError: unsupported configuration: storage type 'dir' requires use of storage format 'fat'

Research is leading me to missing libraries that need installed for Virt-Manager that are not installed by default.

"Not of much help, but from what I'm reading it's not an issue with the compilation of libvirt, optional storage backends where separated from libvirt back in the past (2018?).

You should need the additional package **libvirt-daemon-driver-storage-zfs** installed into unraid."

CITE: https://forums.unraid.net/topic/119932-unable-to-create-zfs-storage-pool-in-libvrt/

### FINDINGS:
Discovered that the settings in the GUI must be bugged or I am doing it wrong... Still need to research that but.

I was able to create a VM using the virt-instll command in CLI.

`sudo virt-install\
`--name ubuntu-guest\
`--os-variant ubuntu20.04\
`--vcpus 2\
`--ram 2048\
`--location http://ftp.ubuntu.com/ubuntu/dists/focal/main/installer-amd64/\
`--network default\
`--video=vmvga\
`--graphics vnc,listen=0.0.0.0\
`--noautoconsole\
`--disk path=/home/scott/VM/test.qcow2,size=30

The above command is surviving a reboot and poweroff. Which means this is a decent workaround for the above issue.


`
