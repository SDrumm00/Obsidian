---
Created:
  - 20240619 8:32 PM
Subject: The ZFS Filesystem
---
-------
Note Links:
- [[ZFS Filesystem]]
- [[Linux]]
-------------
# This file needs cleaned
# Abstract
ZFS on Root on Debian
https://www.youtube.com/watch?v=7F7Ch-ZkiQU

I generally do not recommend ZFS on Root for [[Linux]]. The better way of doing this is by using EXT4 or the default file system for the distro and then using a zpool to store your /home directory on.

At the time of this writing (20231030) I a, using Ubuntu Server with a mirrored root file system and a zpool file system for my /home directory and everything is very robust and and resilient.

[[ZFS Filesystem]] vs [[BTRFS]]
- ZFS wins
 
2023 Learning Goals aligns to my learning more about Linux

ZFS commands
- zpool status - shows status of the pool specified
- zpool clear - removes errors of the specified zpool
- zpool remove - removes any specified devices within the zpool

## Virt-Manager Virtualiztion
For some odd reason, I cannot get the QEMU Virt-Manager application to create storage on a [[ZFS Filesystem]] data set. I get an odd error stating it does not exist.

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

# Abstract
What is [[ZFS]]?

## [[ZFS On Linux]]

## ZFS on Debian
https://wiki.debian.org/ZFS
## TRIM support

[TRIM](https://en.wikipedia.org/wiki/Trim_(computing)) is a kind of command used to inform a the disk device which blocks of data are no longer considered to be 'in use' and therefore can be erased internally, which was introduced soon after SSDs are introduced, and is also widely used on [SMR](https://en.wikipedia.org/wiki/Shingled_magnetic_recording) hard drives. Since the release of OpenZFS 0.8, TRIM support was added.

TRIM is introduced to help the disk controller do better job in garbage collection, and reduce write amplification. Proper trimming may help mitigate/avoid performance degradation, and improve endurance. In real world TRIM is not always vitally necessary, the situation may vary due to different disk controller implementation, flash over provision, and workload patterns. Excessive TRIM could hurt online performance and affect long-term endurance. If your workload constantly write and delete a lot of data (calculated by DPDW), then you might need a higher frequency of TRIM.

Traditional RAIDs (hardware/md) could suffer from performance problems when using TRIM, because there are 2-levels in the path (filesystem - raid), individual TRIM commands are issued in small size like 4KB or 64KB (usually PAGE_SIZE) when reaching the disks, some merging is possible but often difficult to implement. ZFS does not have such kind of issue because the it has direct knowledge to both the file system and space allocation.

### Manual TRIM

To perform TRIM on a pool, use

    zpool trim tank

Status and progress can be viewed with

    zpool status -t tank

Be aware that pool performance could be affected depending on disk performance when workload is heavy.

### Periodic TRIM

On Debian systems, since Bullseye release (or 2.0.3-9 in buster-backports), periodic TRIM is implemented using a custom per-pool property: _org.debian:periodic-trim_

By default, these TRIM jobs are scheduled on the first Sunday of every month. The completion speed depends on the disks size, disk speed and workload pattern. Cheap QLC disks could take considerable more time than very expensive enterprise graded NVMe disks.

|                          |           |      |
| ------------------------ | --------- | ---- |
| org.debian:periodic-trim | Pool SSD  | TRIM |
|                          | nvme-only | yes  |
| auto1                    | sata3.0   | no2  |
| sata>=3.1                | no3       |      |
| mixed                    | no4       |      |
| enabled                  | any       | yes  |
| disabled                 | any       | no   |

1. When _org.debian:periodic-trim_ is not present in pool, or the property is present but value is empty/invalid, they are treated as _auto_.
    
2. SATA SSD with protocol version 3.0 or lower handles TRIM (UNMAP) in synchronous manner which could block all other I/O on the disk immediately until the command is finished, this could lead to severe interruption. In such case, pool trim is only recommended in scheduled maintenance period.
3. SATA SSD with protocol version >=3.1 may perform TRIM in a queued manner, making the operation not blocking. Enabling TRIM on these disks is planned by the Debian ZFS mantainers ([990871](https://bugs.debian.org/990871 "Open in zfs-linux/2.0.3-3: #990871: zfs-linux: periodic-trim should deal with SATA SSD with protocol > 3.0")), but yet to be implemented because there are issues to be considered - for example some disks advertise the ability of doing Queued TRIM although the implementation is [known broken](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/drivers/ata/libata-core.c#n3936). Users can enable the pool trim by setting the property to _enable_ after checking carefully.
    
4. When the >=3.1 support is properly implemented, pool with a mixed types of SSDs will be measured by whether all disks are of the recommended types. Users can enable the pool trim by setting the property to _enable_ after checking all disks in pool carefully.
    

To set the property to _enable_, use:

    zfs set org.debian:periodic-trim=enable tank

Please note this property is set on the root dataset of the pool, not the pool itself because it is not yet implemented.

### autotrim property

ZFS can perform TRIM after data deletion, which is in some way similar to discard mount option in other file systems. To use autotrim, set the pool property:

    zpool set autotrim=on tank

Automatic TRIM looks for space which has been recently freed, and is no longer allocated by the pool, to be periodically trimmed, however it does not immediately reclaim blocks after a free, which makes it very effective at a cost of more likely of encountering tiny ranges.

Note the previous mentioned _periodic-trim_ does not conflict with _autotrim_, and is already sufficient for light usages. For heavy workloads, _periodic-trim_ (which is full trim) can be used to work to complement _autotrim_.

## Scrub Support
Automated scrubbing

Debian by default has a cron job entry to scrub all pools on the second Sunday of every month at 24 minutes past midnight.

See /etc/cron.d/zfsutils-linux and /usr/lib/zfs-linux/scrub for details

It is possible to disable this by setting a zfs user defined property on the root dataset for a pool.

To set the property to _disable_, use:

    zfs set org.debian:periodic-scrub=disable tank
`
