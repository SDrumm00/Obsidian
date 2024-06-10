---
Created:
  - 20240405 3:51 PM
aliases: 
tags:
  - "#document-scanner"
  - brother
  - software
  - drivers
  - firmware
  - linux
Subject: Brother Scanner
Note Links: "[[Linux]]"
---
**NOTE:**
I copied the .rpm and the .deb files to the fileserver in the software backup directory

Storage location is `/home/scott/X/Backups/Software`

[Instructions](https://support.brother.com/g/b/downloadhowto.aspx?c=us&lang=en&prod=ads1000w_us&os=127&dlid=dlf105203_000&flang=4&type3=564)

**How to Install on an RPM based OS**
[[Fedora]], [[Fedora Silverblue]], [[Redhat]], [[CentOS]]
1) Download the driver.

2) Login as a superuser.
    
3)  Install the driver.
	- Turn on your MFC/DCP and connect the USB cable.
	- Open the terminal and go to the directory where the driver is.
	- Install the scanner driver.
	- Command (for rpm) : `rpm  -ihv  --nodeps  (scanner-drivername)`
	- Check if the driver is installed.
	- Command (for rpm) : `rpm  -qa  |  grep  -e  (scanner-drivername)`

**NOTE** 
I had to use [[Distrobox]] to install this rpm file. Also installed simple-scan directly in the [[distrobox]] and exported it as a desktop shortcut. Also had all the dependencies installed with it from the simple-scan website which is a part of Gnome but not installed by default? weirdness...

I don't think the above NOTE is valid any more. Any issues I had have been resolved. Keeping the comment for future reference.

**How to install on a DEB based OS**
[[Ubuntu]], [[Debian ]]

1) Download the driver.  
     
2) Login as a superuser (or use "sudo" option if required) .  
     
3) Install the driver.
    - Turn on your MFC/DCP and connect the USB cable.
    - Open the terminal and go to the directory where the driver is.
    - Install the scanner driver.  
	      - Command (for dpkg) : `dpkg -i --force-all  (scanner-drivername)`
	      - Found that the --force-all argument is invalid and is fine running without it
    - Check if the driver is installed.  
          - Command (for dpkg) : `dpkg -l | grep Brother`

