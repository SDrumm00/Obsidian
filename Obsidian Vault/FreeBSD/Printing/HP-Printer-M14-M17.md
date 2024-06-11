# Printing in [[FreeBSD]]

OS Version: FreeBSD 14-Release

Date: 202040116

Author: Scott Drumm

Tags: #Unix #Linux #Hewlett-Packard

Citations: 
- https://docs.freebsd.org/en/books/handbook/printing/#printing-other
  - Section 11.6.2. HPLIP
- https://cgit.freebsd.org/ports/tree/print/hplip/
  - Install HPLIP for HP Printers

## Steps to make Printing work on FreeBSD

### Step 1: Intro and Install the Software

#### An Introduction to the Common Unix Printing System (CUPS)

CUPS, the Common UNIX Printing System, provides a portable printing layer for UNIX®-based operating systems. It has been developed by Easy Software Products to promote a standard printing solution for all UNIX® vendors and users.

CUPS uses the Internet Printing Protocol (IPP) as the basis for managing print jobs and queues. The Line Printer Daemon (LPD), Server Message Block (SMB), and AppSocket (aka JetDirect) protocols are also supported with reduced functionality. CUPS adds network printer browsing and PostScript Printer Description (PPD) based printing options to support real-world printing under UNIX®. As a result, CUPS is ideally-suited for sharing and accessing printers in mixed environments of FreeBSD, Linux®, Mac OS® X, or Windows®.

The main site for CUPS is http://www.cups.org/.

#### Installing the CUPS Print Server

To install CUPS using a precompiled binary, issue the following command from a root terminal:

`# pkg install cups`

### Step 2: Configure the CUPS Print Server

After installation, a few files must be edited to configure the CUPS server. First, create or modify, as the case may be, the file /etc/devfs.rules and add the following information to set the proper permissions on all potential printer devices and to associate printers with the cups user group:

```
[system=10]
add path 'unlpt*' mode 0660 group cups
add path 'ulpt*' mode 0660 group cups
add path 'lpt*' mode 0660 group cups
add path 'usb/X.Y.Z' mode 0660 group cups
```

Note that X, Y, and Z should be replaced with the target USB device listed in the /dev/usb directory that corresponds to the printer. To find the correct device, examine the output of dmesg(8), where ugenX.Y lists the printer device, which is a symbolic link to a USB device in /dev/usb.
* For me at the time it was  the follow steps to identify the HP USB device
  * `dmesg | grep HP`
    ```
    Timecounter "HPET" frequency 14318180 Hz quality 950
    Event timer "HPET" frequency 14318180 Hz quality 350
    Event timer "HPET1" frequency 14318180 Hz quality 350
    Event timer "HPET2" frequency 14318180 Hz quality 350
    ugen1.4: <HP HP LaserJet M14-M17> at usbus1
    ```

Next, add two lines to /etc/rc.conf as follows:

`doas sysrc cupsd_enable="YES"`

`doas sysrc devfs_system_ruleset="system"`

These two entries will start the CUPS print server on boot and invoke the local devfs rule created above, respectively.

### Step 3: Configuring Printers on the CUPS Print Server

After the CUPS system has been installed and configured, the administrator can begin configuring the local printers attached to the CUPS print server. This part of the process is very similar, if not identical, to configuring CUPS printers on other UNIX®-based operating systems, such as a Linux® distribution.

The primary means for managing and administering the CUPS server is through the web-based interface, which can be found by launching a web browser and entering http://localhost:631 in the browser’s URL bar. If the CUPS server is on another machine on the network, substitute the server’s local IP address for localhost. The CUPS web interface is fairly self-explanatory, as there are sections for managing printers and print jobs, authorizing users, and more. Additionally, on the right-hand side of the Administration screen are several check-boxes allowing easy access to commonly-changed settings, such as whether to share published printers connected to the system, whether to allow remote administration of the CUPS server, and whether to allow users additional access and privileges to the printers and print jobs.

Adding a printer is generally as easy as clicking "Add Printer" at the Administration screen of the CUPS web interface, or clicking one of the "New Printers Found" buttons also at the Administration screen. When presented with the "Device" drop-down box, simply select the desired locally-attached printer, and then continue through the process. If one has added the print/gutenprint-cups or print/hplip ports or packages as referenced above, then additional print drivers will be available in the subsequent screens that might provide more stability or features.

### Step 4: Install Additional Dependencies (If Needed)

For me, they were needed.

#### HPLIP

Hewlett Packard provides a printing system that supports many of their inkjet and laser printers. The port is print/hplip. The main web page is at https://developers.hp.com/hp-linux-imaging-and-printing. The port handles all the installation details on FreeBSD. Configuration information is shown at https://developers.hp.com/hp-linux-imaging-and-printing/install.

I chose to simply install the missing binaries with the property driver software.

`doas pkg install hplip`

Then reboot the computer and all services automatically start with bootup and you should be able to navigate back to CUPS and configure the device with the proper drivers if they were needed.

After this setup, I was able to print a test page successfully and move on with my life.


## Steps to make Printing work on Debian
In [[Debian]] we set the [[printer]]  up in the following way.

See here => https://wiki.debian.org/SystemPrinting

## Introduction

The Debian printing system has undergone many significant changes over the past few years, with much of the [printer management](https://wiki.debian.org/CUPSPrintQueues) taking advantage of the advances in [modern printer](https://wiki.debian.org/CUPSDriverlessPrinting#intro) technology and the proliferation of [IPP printers](https://wiki.debian.org/PrintingGlossaryandIndex#ipp). [Modern printers](https://wiki.debian.org/CUPSQuickPrintQueues#intro) are catered for by CUPS and supported by [OpenPrinting](https://openprinting.github.io/) initiatives such as [cups-filters](https://openprinting.github.io/projects/). Notwithstanding this, there are still many legacy (non-modern) printers and their [drivers](https://openprinting.org/drivers) in use. This page and [another](https://wiki.debian.org/CUPSPrintQueues) intend to cater for users with both types of printer device.

Legacy (non-modern) printers require printer drivers. Should a user be unsure what is suitable for the printer at hand, a comprehensive set of [free PPDs](https://wiki.debian.org/CUPSPrintQueues#ppd) and drivers would be put on the system with

`apt install printer-driver-all`

Depending on the printer, it may also be desirable to install one or more of the following:

- [foomatic-db-engine](https://packages.debian.org/foomatic-db-engine "DebianPkg").
- [hp-ppd](https://packages.debian.org/hp-ppd "DebianPkg").
- [openprinting-ppds](https://packages.debian.org/openprinting-ppds "DebianPkg").

The [OpenPrinting website](https://openprinting.org/printers) is a good source of information for matching printers with [free](https://www.debian.org/social_contract) drivers. For a printer that requires a non-free driver a user would have to see [what the manufacturer has to offer](https://wiki.debian.org/CUPSPrintQueues#nonfree).

### Driverless Printing

A proportion of the material on the [Printing Portal](https://wiki.debian.org/Printing) pages is applicable to installing [printer drivers](https://wiki.debian.org/CUPSPrintQueues#driver) (free and [non-free](https://wiki.debian.org/CUPSPrintQueues#nonfree)) and [PPDs](https://wiki.debian.org/CUPSPrintQueues#ppd) and setting up a [print queue](https://wiki.debian.org/CUPSPrintQueues#printqueue) for legacy printers. However, it is as well to be aware that drivers and PPDs are [deprecated in CUPS](https://github.com/OpenPrinting/cups/issues/103) and eventually they will not be catered for as they are now. This has been a long-term objective of the CUPS project for some time.

Users possessing a [modern printer](https://wiki.debian.org/SystemPrinting#intro) are urged to consider the following points and explore a [driverless printing solution](https://wiki.debian.org/CUPSDriverlessPrinting) for their printing needs, whether or not the deprecation is a motivating factor,.

- Driverless printing was introduced to [CUPS](https://wiki.debian.org/CUPSDriverlessPrinting#shortlpadmin) and [cups-browsed](https://wiki.debian.org/CUPSDriverlessPrinting#shortcupsbrowsed) in Debian 9 (stretch).
    
- Support for driverless printing with [CUPS](https://wiki.debian.org/CUPSDriverlessPrinting#shorttempq) and [cups-browsed](https://wiki.debian.org/CUPSDriverlessPrinting#cupsbrowsed) is considerably extended in [Debian 10 (buster)](https://wiki.debian.org/CUPSQuickPrintQueues) and [Debian 11 (bullseye)](https://wiki.debian.org/CUPSDriverlessPrinting#debian).
    
- Printers sold in the last 10 years or so are almost always [AirPrint](https://wiki.debian.org/AirPrint) devices and therefore would [support driverless printing](https://wiki.debian.org/CUPSQuickPrintQueues#intro) when the device is connected by ethernet or wireless. Additionally, a USB connected modern printer might be capable of driverless printing if it is [IPP-over-USB-capable](https://wiki.debian.org/DriverlessPrinting#ippoverusb)    

### Software Installation

CUPS and cups-filters are central to the printing system and both are installed with

`apt install cups`

This installation provides a functional printing system that is entirely suitable for use with a [modern printer](https://wiki.debian.org/SystemPrinting#driverless) without any further software installation. Legacy printers would require a [driver installation](https://wiki.debian.org/SystemPrinting#intro).

- [cups-browsed](https://wiki.debian.org/CUPSPrintQueues#cupsbrowsed) is installed as a recommended package.
    
- Modern [USB](https://wiki.debian.org/CUPSDriverlessPrinting#ippoverusb) and [ethernet and wireless](https://wiki.debian.org/CUPSQuickPrintQueues) connected printers on Debian 11 should be detected and auto-setup by cups-browsed.
    
- Debian 10 will handle a modern USB connected printer driverlessly by following [this ipp-usb advice](https://wiki.debian.org/CUPSDriverlessPrinting#summary). Otherwise, [a driver](https://wiki.debian.org/SystemPrinting#intro) will be required.
    
- Modern IPP printers are printers capable of [driverless printing](https://wiki.debian.org/SystemPrinting#driverless); that is, free or non-free vendor packages or plugins are not required.
    

Alternatively, should it be thought necessary or desirable, a user may manually install a print queue (remote or local) with [lpadmin](https://wiki.debian.org/CUPSPrintQueues#lpadmin), the [web interface](https://wiki.debian.org/SystemPrinting#webinterface) of CUPS or [system-config-printer](https://wiki.debian.org/SystemPrinting#scp). Successful printing is usually ensured if a modern printer is [set up as a driverless printer](https://wiki.debian.org/CUPSDriverlessPrinting) or a legacy printer is supported by one of the [installed packages](https://wiki.debian.org/SystemPrinting#intro).

### Summary

- Do you have a [modern or a legacy](https://wiki.debian.org/SystemPrinting#intro) printer? Is there a [driver](https://wiki.debian.org/SystemPrinting#intro) for your legacy printer? Is your modern printer using facilities on buster or bulleye? Do you [appreciate](https://wiki.debian.org/SystemPrinting#driverless) that bullseye handles USB and network connected modern printers whereas buster deals only with networked modern printers? Have you [checked](https://wiki.debian.org/SystemPrinting#utilities) whether cups-browsed has auto-setup a working print queue before making alterations to the system? Are you fully aware that your USB connected modern printer [shouldn't be set up](https://wiki.debian.org/SystemPrinting#local) as a USB printer if it is capable of IPP-over-USB? Why are you using HPLIP and its utilities if you have a modern printer?
    
- A modern printer or [MFD](https://wiki.debian.org/PrintingGlossaryandIndex#mfd) on bullseye should not require a driver, [even for scanning](https://wiki.debian.org/SaneOverNetwork#escl). Packages such as [HPLIP](https://packages.debian.org/HPLIP "DebianPkg") and [printer-driver-escpr](https://packages.debian.org/printer-driver-escpr "DebianPkg") need not be on the system.
    
- A legacy printer device will require a [PPD](https://wiki.debian.org/SystemPrinting#intro), and probably a driver too.
    
- A print queue for a modern printer may be [set up manually](https://wiki.debian.org/SystemPrinting#utilities) if cups-browsed is not installed or there are issues with its behaviour.

## Additional Information

[CUPS](https://en.wikipedia.org/wiki/CUPS "WikiPedia") is a printing system intended as a replacement for the [Berkeley](https://en.wikipedia.org/wiki/Berkeley_printing_system "WikiPedia") and [System V](https://en.wikipedia.org/wiki/System_V_printing_system "WikiPedia") printing systems. It was created by Easy Software Products (ESP) but today is owned and, [in the recent past](https://wiki.debian.org/Printing#important), was developed and maintained by [Apple Inc](https://www.apple.com/), with the original owner of ESP, Michael R. Sweet, being Senior Printing System Engineer until [December 2019](https://lists.linuxfoundation.org/pipermail/printing-architecture/2019/003767.html). In the days of ESP ownership CUPS was officially known as the Common UNIX Printing System.

The CUPS printing system used in Debian involves complex interactions between the programs contained in many packages. The scheduler, [cupsd](https://packages.debian.org/cups-daemon "DebianPkg"), is at the heart of the system, but support from a [filtering system](https://packages.debian.org/cups-filters "DebianPkg") and a large array of packages is necessary for successful, effortless printing on a wide variety of printers. Having said that, the system appears to work well and there is little sign that Debian users have any great difficulty in setting up local or networked printers and getting them to function correctly. This page is intended to help maintain that situation.

CUPS is the default printing system for Debian but the Berkeley LPR software is also in the archives as [lprng](https://packages.debian.org/lprng "DebianPkg"). It cannot coexist with cups on the same system.


## Known Issues
I encountered and error after setting up CUPS and the HP driver.

Cups "add printer" page returns forbidden on Web Interface on `http://localhost:631/admin`

https://unix.stackexchange.com/questions/235477/cups-add-printer-page-returns-forbidden-on-web-interface

### Resolution
I had to add myself as a user with correct permissions to the "lpadmin" group so that I could connect to the printer socket and add printers to my config.

`sudo usermod -a -G lpadmin scott`