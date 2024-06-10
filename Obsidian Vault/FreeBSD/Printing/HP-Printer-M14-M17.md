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

Fi


[//begin]: # "Autogenerated link references for markdown compatibility"
[FreeBSD]: ..%2FFreeBSD.md "FreeBSD"
[//end]: # "Autogenerated link references"
