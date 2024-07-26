---
Created:
  - 20240723 8:50 PM
Subject: Sanoid for ZFS
tags:
  - ZFS
---
----------------
Note Links:
- [[ZFS Snapshots]]
- [[ZFS Filesystem]]
--------------------

# Abstract
- https://github.com/jimsalterjrs/sanoid

Sanoid is a policy-driven snapshot management tool for ZFS filesystems. When combined with the Linux KVM hypervisor, you can use it to make your systems [functionally immortal](https://openoid.net/transcend) via automated snapshot management and over-the-air replication.

# Installation

**Sanoid** and **Syncoid** are complementary but separate pieces of software. To install and configure them, follow the guide below for your operating system. Everything in `code blocks` should be copy-pasteable. If your OS isn't listed, a set of general instructions is at the end of the list and you can perform the process manually.

## Debian/Ubuntu

Install prerequisite software:

```shell
apt install debhelper libcapture-tiny-perl libconfig-inifiles-perl pv lzop mbuffer build-essential git
```

Clone this repo, build the debian package and install it (alternatively you can skip the package and do it manually like described below for CentOS):

```shell
git clone https://github.com/jimsalterjrs/sanoid.git
cd sanoid
# checkout latest stable release or stay on master for bleeding edge stuff (but expect bugs!)
git checkout $(git tag | grep "^v" | tail -n 1)
ln -s packages/debian .
dpkg-buildpackage -uc -us
sudo apt install ../sanoid_*_all.deb
```

Enable sanoid timer:

```shell
# enable and start the sanoid timer
sudo systemctl enable --now sanoid.timer
```

# Configuration

**Sanoid** won't do anything useful unless you tell it how to handle your ZFS datasets in `/etc/sanoid/sanoid.conf`.

**Syncoid** is a command line utility that doesn't require any configuration, with all of its switches set at runtime.

## Sanoid

Take a look at the files `sanoid.defaults.conf` and `sanoid.conf` for all possible configuration options.

Also have a look at the README.md for a simpler suggestion for `sanoid.conf`.

## Syncoid

If you are pushing or pulling from a remote host, create a user with privileges to `ssh` as well as `sudo`. To ensure that `zfs send/receive` can execute, adjust the privileges of the user to execute `sudo` **without** a password for only the `zfs` binary (run `which zfs` to find the path of the `zfs` binary). Modify `/etc/sudoers` by running `# visudo`. Add the following line for your user.