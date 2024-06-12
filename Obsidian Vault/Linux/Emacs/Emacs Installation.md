---
Created:
  - 20240311 3:32 PM
aliases: 
tags:
  - Building-A-Second-Brain
  - System-Crafters
  - IDE
  - developer
  - linux
  - Org-Mode
  - Org-Roam
  - GNU
Subject: GNU Emacs Learning
---
-----------------------------
# Downloading & Installing [[Emacs]]

[https://www.gnu.org/software/emacs/download.html#gnu-linux](https://www.gnu.org/software/emacs/download.html#gnu-linux)

At the time of writing this document, I am using [[Debian]] [[Linux]](app://obsidian.md/Linux/Debian/Debian%20Linux) as my distro of choice so these instructions are tailored for that OS.

Emacs runs on several operating systems regardless of the machine type. The main ones are: GNU, GNU/Linux, [[FreeBSD]], NetBSD, OpenBSD, MacOS, MS Windows and Solaris.

You can download GNU Emacs releases from a [nearby GNU mirror](http://ftpmirror.gnu.org/emacs/); or if automatic redirection does not work see the list of [GNU mirrors](https://www.gnu.org/order/ftp.html), or use the [main GNU ftp](http://ftp.gnu.org/gnu/emacs) server.

GNU Emacs source code and development is hosted on [savannah.gnu.org](http://savannah.gnu.org/projects/emacs/).

#### 

GNU/Linux

Most GNU/Linux distributions provide GNU Emacs in their repositories, which is the recommended way to install Emacs unless you always want to use the latest release.

If your GNU/Linux distribution includes a graphical package manager, this is often the easiest way to install Emacs. Please refer to the documentation for your distribution for full instructions on how to install a package.

You might prefer installing from the command-line. Here are some of the commands that various GNU/Linux distributions use to install a package named `emacs`.

```
guix package -i emacs
```

```
sudo apt-get install emacs
```

```
sudo pacman -S emacs
```

```
sudo dnf install emacs
```

```
sudo zypper install emacs
```

On some systems, the package name must include the Emacs version number, such as `emacs-27.1`, instead of `emacs`.

#### 

BSDs

The BSDs provide GNU Emacs in their repositories, which is the recommended way to install Emacs unless you always want to use the latest release.

#### 

Nonfree systems

The reason for GNU Emacs's existence is to provide a powerful editor for the [GNU operating system](https://www.gnu.org/gnu). Versions of GNU, such as [GNU/Linux](https://www.gnu.org/gnu/linux-and-gnu.html), are the primary platforms for Emacs development.

However, GNU Emacs includes support for some other systems that volunteers choose to support.

The purpose of the GNU system is to [give users the freedom that proprietary software takes away from its users](https://www.gnu.org/philosophy/free-software-even-more-important.html). Proprietary operating systems (like other proprietary programs) are an injustice, and we aim for a world in which they do not exist.

To improve the use of proprietary systems is a misguided goal. Our aim, rather, is to eliminate them. We include support for some proprietary systems in GNU Emacs in the hope that running Emacs on them will give users a taste of freedom and thus lead them to free themselves.

#### 

Windows

GNU Emacs for Windows can be downloaded from a [nearby GNU mirror](http://ftpmirror.gnu.org/emacs/windows); or the [main GNU FTP server](http://ftp.gnu.org/gnu/emacs/windows/).  
Mostly simply, download and run the emacs-`version`-installer.exe which will install Emacs and create shortcuts for you. Alternately, download emacs-`version`.zip then unzip, preserving the directory structure. You can then run `bin\runemacs.exe` or create a desktop shortcut to `bin\runemacs.exe` and start Emacs by double-clicking on that shortcut's icon.

The Windows binaries are signed by Corwin Brust `ECE7 7CF4 17C7 6C1A CFCE 7C2B 5B61 3551 1580 F007`.

MSYS2 users can install Emacs (64bits build) with the following:

```
pacman -S mingw-w64-x86_64-emacs
```

For the 32bits build, run:

```
pacman -S mingw-w64-i686-emacs
```

#### 

MacOS

Emacs can be installed on MacOS using [Homebrew](http://brew.sh/).

```
brew install --cask emacs
```

Using [MacPorts](https://www.macports.org/):

```
sudo port install emacs-app
```

The [Emacs for OSX](https://emacsformacosx.com/) website also provides universal binaries.

#### 

Post Installation

Following the above sections, it installs without issue on Debian and the details of the installation are as follows:

Get the version of the package post-installation:

```
emacs -version
```

output:

```
GNU Emacs 28.2
```