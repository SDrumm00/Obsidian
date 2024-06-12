# FreeBSD Bhyve

Author: Scott Drumm

Date: 20240118

Tags: #FreeBSD #Linux #Virtualization #Hypervisors

Citations:
- https://docs.freebsd.org/en/books/handbook/virtualization/


## Virtualization within [[FreeBSD]]

__Abstract__

Virtualization software allows
multiple operating systems to run   simultaneously on the same
computer. Such software systems for PCs often involve a host
operating system which runs the virtualization software and supports
any number of guest operating systems.

In this document, we will explore the use case where
the host machine is FreeBSD and the guests are running VM's within
bhyve. We will deploy a [[Linux 1]] guest within bhyve in order to
have a working environment for testing purposes.

## Bhyve History
The bhyve BSD-licensed hypervisor became part of the base system with FreeBSD 10.0-RELEASE. This hypervisor supports a number of guests, including FreeBSD, OpenBSD, and many [[Linux 1]]® distributions. By default, bhyve provides access to serial console and does not emulate a graphical console. Virtualization offload features of newer CPUs are used to avoid the legacy methods of translating instructions and manually managing memory mappings.

The bhyve design requires a processor that supports Intel® Extended Page Tables (EPT) or AMD® Rapid Virtualization Indexing (RVI) or Nested Page Tables (NPT). Hosting Linux® guests or FreeBSD guests with more than one vCPU requires VMX unrestricted mode support (UG). Most newer processors, specifically the Intel® Core™ i3/i5/i7 and Intel® Xeon™ E3/E5/E7, support these features. UG support was introduced with Intel’s Westmere micro-architecture. For a complete list of Intel® processors that support EPT, refer to https://ark.intel.com/content/www/us/en/ark/search/featurefilter.html?productType=873&0_ExtendedPageTables=True. RVI is found on the third generation and later of the AMD Opteron™ (Barcelona) processors. The easiest way to tell if a processor supports bhyve is to run dmesg or look in /var/run/dmesg.boot for the POPCNT processor feature flag on the Features2 line for AMD® processors or EPT and UG on the VT-x line for Intel® processors.

## Getting Started
### Step 1: Preparing the Host
The first step to creating a virtual machine in bhyve is configuring the host system. First, load the bhyve kernel module:

Input:

    # kldload vmm

Then, create a tap interface for the network device in the virtual machine to attach to. In order for the network device to participate in the network, also create a bridge interface containing the tap interface and the physical interface as members. In this example, the physical interface is igb0:

    # ifconfig tap0 create
    # sysctl net.link.tap.up_on_open=1
    net.link.tap.up_on_open: 0 -> 1
    # ifconfig bridge0 create
    # ifconfig bridge0 addm igb1 addm tap0
    # ifconfig bridge0 up

  *NOTE:** The igbX matters here since on my host machine I have multiple ethernet connections.

### Step 2.0: Creating a FreeBSD Guest

Create a file to use as the virtual disk for the guest machine. Specify the size and name of the virtual disk:

    # truncate -s 16G freebsdguest.img

Download an installation image of FreeBSD to install:

    # fetch https://download.freebsd.org/releases/ISO-IMAGES/14.0/FreeBSD-14.0-RELEASE-amd64-bootonly.iso
    FreeBSD-14.0-RELEASE-amd64-bootonly.iso                426 MB 4665 kBps 01m34s

FreeBSD comes with an example script for running a virtual machine in bhyve. The script will start the virtual machine and run it in a loop, so it will automatically restart if it crashes. The script takes a number of options to control the configuration of the machine: -c controls the number of virtual CPUs, -m limits the amount of memory available to the guest, -t defines which tap device to use, -d indicates which disk image to use, -i tells bhyve to boot from the CD image instead of the disk, and -I defines which CD image to use. The last parameter is the name of the virtual machine, used to track the running machines. This example starts the virtual machine in installation mode:

    # sh /usr/share/examples/bhyve/vmrun.sh -c 1 -m 1024M -t tap0 -d freebsdguest.img -i -I FreeBSD-14.0-RELEASE-amd64-bootonly.iso freebsd

The virtual machine will boot and start the installer. After installing a system in the virtual machine, when the system asks about dropping in to a shell at the end of the installation, choose Yes.

Reboot the virtual machine. While rebooting the virtual machine causes bhyve to exit, the vmrun.sh script runs bhyve in a loop and will automatically restart it. When this happens, choose the reboot option from the boot loader menu in order to escape the loop. Now the guest can be started from the virtual disk:

    # sh /usr/share/examples/bhyve/vmrun.sh -c 4 -m 1024M -t tap0 -d freebsdguest.img freebsd

## Step 2.1: Creating a [[Linux]]® Guest

**Dependencies:**

In order to boot operating systems other than FreeBSD, the sysutils/grub2-bhyve port must be first installed.

    # pkg install grub2-bhyve 

Next, create a file to use as the virtual disk for the guest machine:

    # truncate -s 16G linux.img

Starting a virtual machine with bhyve is a two step process. First a kernel must be loaded, then the guest can be started. The Linux® kernel is loaded with sysutils/grub2-bhyve. Create a device.map that grub will use to map the virtual devices to the files on the host system:

    (hd0) ./linux.img
    (cd0) ./somelinux.iso -- this is the Linux of choice here such as [[Ubuntu]][[Debian]] or [[Arch]]

Use sysutils/grub2-bhyve to load the Linux® kernel from the ISO image: [[Debian]]

    # grub-bhyve -m device.map -r cd0 -M 1024M linuxguest

This will start grub. If the installation CD contains a grub.cfg, a menu will be displayed. If not, the vmlinuz and initrd files must be located and loaded manually:

    grub> ls
    (hd0) (cd0) (cd0,msdos1) (host)
    grub> ls (cd0)/isolinux
    boot.cat boot.msg grub.conf initrd.img isolinux.bin isolinux.cfg memtest
    splash.jpg TRANS.TBL vesamenu.c32 vmlinuz
    grub> linux (cd0)/isolinux/vmlinuz
    grub> initrd (cd0)/isolinux/initrd.img
    grub> boot

Now that the Linux® kernel is loaded, the guest can be started:

    # bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap0 -s 3:0,virtio-blk,./linux.img \
    -s 4:0,ahci-cd,./somelinux.iso -l com1,stdio -c 4 -m 1024M linuxguest

The system will boot and start the installer. After installing a system in the virtual machine, reboot the virtual machine. This will cause bhyve to exit. The instance of the virtual machine needs to be destroyed before it can be started again:

    # bhyvectl --destroy --vm=linuxguest

Now the guest can be started directly from the virtual disk. Load the kernel:

    # grub-bhyve -m device.map -r hd0,msdos1 -M 1024M linuxguest
    grub> ls
    (hd0) (hd0,msdos2) (hd0,msdos1) (cd0) (cd0,msdos1) (host)
    (lvm/VolGroup-lv_swap) (lvm/VolGroup-lv_root)
    grub> ls (hd0,msdos1)/
    lost+found/ grub/ efi/ System.map-2.6.32-431.el6.x86_64 config-2.6.32-431.el6.x
    86_64 symvers-2.6.32-431.el6.x86_64.gz vmlinuz-2.6.32-431.el6.x86_64
    initramfs-2.6.32-431.el6.x86_64.img
    grub> linux (hd0,msdos1)/vmlinuz-2.6.32-431.el6.x86_64 root=/dev/mapper/VolGroup-lv_root
    grub> initrd (hd0,msdos1)/initramfs-2.6.32-431.el6.x86_64.img
    grub> boot

Boot the virtual machine:

    # bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap0 \
    -s 3:0,virtio-blk,./linux.img -l com1,stdio -c 4 -m 1024M linuxguest

Linux® will now boot in the virtual machine and eventually present you with the login prompt. Login and use the virtual machine. When you are finished, reboot the virtual machine to exit bhyve. Destroy the virtual machine instance:

    # bhyvectl --destroy --vm=linuxguest

## Step 2.2:  Booting bhyve Virtual Machines with UEFI Firmware

In addition to bhyveload and grub-bhyve, the bhyve hypervisor can also boot virtual machines using the UEFI userspace firmware. This option may support guest operating systems that are not supported by the other loaders.

In order to make use of the UEFI support in bhyve, first obtain the UEFI firmware images. This can be done by installing sysutils/bhyve-firmware port or package.

With the firmware in place, add the flags -l bootrom,/path/to/firmware to your bhyve command line. The actual bhyve command may look like this:

    # bhyve -AHP -s 0:0,hostbridge -s 1:0,lpc \
    -s 2:0,virtio-net,tap1 -s 3:0,virtio-blk,./disk.img \
    -s 4:0,ahci-cd,./install.iso -c 4 -m 1024M \
    -l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI.fd \
    guest

sysutils/bhyve-firmware also contains a CSM-enabled firmware, to boot guests with no UEFI support in legacy BIOS mode:

    # bhyve -AHP -s 0:0,hostbridge -s 1:0,lpc \
    -s 2:0,virtio-net,tap1 -s 3:0,virtio-blk,./disk.img \
    -s 4:0,ahci-cd,./install.iso -c 4 -m 1024M \
    -l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI_CSM.fd \
    guest

## Step 2.3: Graphical UEFI Framebuffer for bhyve Guests

The UEFI firmware support is particularly useful with predominantly graphical guest operating systems such as Microsoft Windows®.

Support for the UEFI-GOP framebuffer may also be enabled with the -s 29,fbuf,tcp=0.0.0.0:5900 flags. The framebuffer resolution may be configured with w=800 and h=600, and bhyve can be instructed to wait for a VNC connection before booting the guest by adding wait. The framebuffer may be accessed from the host or over the network via the VNC protocol. Additionally, -s 30,xhci,tablet can be added to achieve precise mouse cursor synchronization with the host.

The resulting bhyve command would look like this:

    # bhyve -AHP -s 0:0,hostbridge -s 31:0,lpc \
    -s 2:0,virtio-net,tap1 -s 3:0,virtio-blk,./disk.img \
    -s 4:0,ahci-cd,./install.iso -c 4 -m 1024M \
    -s 29,fbuf,tcp=0.0.0.0:5900,w=800,h=600,wait \
    -s 30,xhci,tablet \
    -l bootrom,/usr/local/share/uefi-firmware/BHYVE_UEFI.fd \
    guest

Note, in BIOS emulation mode, the framebuffer will cease receiving updates once control is passed from firmware to guest operating system.
## Step 2.4: Using ZFS with bhyve Guests

If ZFS is available on the host machine, using ZFS volumes instead of disk image files can provide significant performance benefits for the guest VMs. A ZFS volume can be created by:

    # zfs create -V16G -o volmode=dev zroot/linuxdisk0

When starting the VM, specify the ZFS volume as the disk drive:

    # bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap0 -s3:0,virtio-blk,/dev/zvol/zroot/linuxdisk0 \
    -l com1,stdio -c 4 -m 1024M linuxguest

## Step 2.5: Virtual Machine Consoles

It is advantageous to wrap the bhyve console in a session management tool such as sysutils/tmux or sysutils/screen in order to detach and reattach to the console. It is also possible to have the console of bhyve be a null modem device that can be accessed with cu. To do this, load the nmdm kernel module and replace -l com1,stdio with -l com1,/dev/nmdm0A. The /dev/nmdm devices are created automatically as needed, where each is a pair, corresponding to the two ends of the null modem cable (/dev/nmdm0A and /dev/nmdm0B). See nmdm(4) for more information.

    # kldload nmdm
    # bhyve -A -H -P -s 0:0,hostbridge -s 1:0,lpc -s 2:0,virtio-net,tap0 -s 3:0,virtio-blk,./linux.img \
        -l com1,/dev/nmdm0A -c 4 -m 1024M linuxguest
    # cu -l /dev/nmdm0B
    Connected

    Ubuntu 13.10 handbook ttyS0

    handbook login:

## Step 3: Managing Virtual Machines

A device node is created in /dev/vmm for each virtual machine. This allows the administrator to easily see a list of the running virtual machines:

    # ls -al /dev/vmm
    total 1
    dr-xr-xr-x   2 root  wheel    512 Mar 17 12:19 ./
    dr-xr-xr-x  14 root  wheel    512 Mar 17 06:38 ../
    crw-------   1 root  wheel  0x1a2 Mar 17 12:20 guestname
    crw-------   1 root  wheel  0x19f Mar 17 12:19 linuxguest
    crw-------   1 root  wheel  0x1a1 Mar 17 12:19 otherguest

A specified virtual machine can be destroyed using bhyvectl:

    # bhyvectl --destroy --vm=guestname

## Step 4: Persistent Configuration

In order to configure the system to start bhyve guests at boot time, the following configurations must be made in the specified files:

    /etc/sysctl.conf

    net.link.tap.up_on_open=1

    /etc/rc.conf

    cloned_interfaces="bridge0 tap0"
    ifconfig_bridge0="addm igb0 addm tap0"
    kld_list="nmdm vmm"

fi





[//begin]: # "Autogenerated link references for markdown compatibility"
[FreeBSD]: FreeBSD.md "FreeBSD"
[Linux]: Linux/Linux.md "Linux"
[//end]: # "Autogenerated link references"
