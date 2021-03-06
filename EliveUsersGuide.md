---
title: "AbantOS Linux Users Guide (Elive/Debian)"
author: [See Credits]
date: 2017-11-05
subject: "CSCI 2461-70/2482-40 Fall 2017 Final Project"
tags: [AbantOS, Elive, Linux, Incident Handling, Final Project]
titlepage: true
titlpage-color: "D8DE2C"
titlepage-text-color: "5F5F5F"
abstract: "AbantOS Linux, based on Elive/Debian with the Englightenment Window Manager/GUI. Created by and in support of the Fall 2017 Saint Paul College CSCI 2461-70 Computer Networking 3 - Linux, and 2482-40 Incident Handling & Disaster Recovery classes. "
...

# Collaborative 2461-70/2482-40 Fall 2017 Final Project

## Back Story: Build Your Own Linux

During Week 10 of CSCI 2461-70 and CSCI 2482-40, after significant discussion all students enrolled in these classes unanimiously decided to join efforts and chose as a Collaborative Class Final Project **building their own Specialized Linux distribution** based on Debian Linux and the Elive project. CSI 2461-70 is focusing on educational components of Linux, and CSCI 2482-40 is focusing on Incident Handling and Disaster Recovery components.   

This document serves as a Users Guide to the 2482-40 and 2461-70 classes and associated tools, techniques and processes. 

## Project Name: AbantOS Linux

In the African Bantu group Abantos means "Belongs to Human Beings." 

AbantOS Linux is based on and created in collaboration with Thanatermesis of the eLiveCD Project. 

The name AbantOS is partially an homage to Ubuntu Linux.  

## Documentation Generation

This document uses the [Eisvogel.tex Template](https://github.com/Wandmalfarbe/pandoc-latex-template) and Pandoc, using a similar method to [OWASP's Top 10](https://github.com/OWASP/Top10/tree/master/2017) [method](https://github.com/OWASP/Top10/blob/master/2017/generate_document.sh)

~~~shell
$ pandoc AbantOS-Elive-UsersGuide-17319-835.md -o ./AbantOS-Elive-UsersGuide-17319-835.pdf --from markdown+multiline_tables --template eisvogel.tex --listings --toc
~~~

## Copyright & License


This work is licensed [Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

All contributors hold their original Copyright 2017.

![Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](AbantOS-Elive-UsersGuide-17319-CC-BY-SA-v4-Intl-88x31.png)



# Introduction

## Social Dynamics of Debian

* The key points to remember in considering Debianos social dynamics as applied to becoming a package maintainer are these:

    - Debian is a volunteer-powered project, and self-motivation and friendly cooperation are crucial.
    - Debianos development is consistently evolving, and maintainers must adapt along with it. This adaptation must be self-driven; you canot rely on others to bring you along.

## Programs Needed for Development

Though many if not all packages necessary for development should come installed with the base Debian installation, itos good to check they are present with *aptitude show package* or *dpkg -s package*. The most important package to make sure is installed is *build-essential*. It should pull in other packages that are necessary for the build environment if theyore not already installed.

Other packages are some measure of helpful and/or necessary for building particular packages down the road. Some are particular compilers for specific languages (i.e. *gfortran* or *gpc*) or packages for scripting languages themselves (i.e. *perl* and *python*). Others make parts of the process easier (such as the *debhelper/dh-make* packages or the *fakeroot* tool) or allow checking for errors after the build (*lintian*). The full range of additional build packages listed should be installed and understood before proceeding.

## Documentation needed for development

This section of chapter 1 includes the debian-policy which is the explanations of the structures and contents of the Debian archive and several OS design issues. It also includes requirements that each package must satisfy to be included in the distribution.  

Important documentations that needed to be read for the development are auto tutorial and gnu-standards.  If any errors or bugs occur, the maint-guide package can be used to report using report bug.

## Where to ask for help?

There are a few documentations that will help solve any problems we encounter during installing packages or using any commands. Available sources to solve any issues are; websites such as lists.debian.org or wiki.debian.org/Teams.

# Debian Package Building 

## Workflow

    1. Get a copy of the upstream software, usually in a compressed tar format.
        * package-version.tar.gz
    2. Add Debian-specific packaging medications to the upstream program under the Debian directory, and create a non-native source package.

        * package_version.orig.tar.gz
        * package_version-revision.debian.tar.gz
        * package_version-revision.dsc

    3. Build Debian binary packages, which are ordinary installable package files in .deb format. The character separating package and version change from hyphen to underscore. As the following command.

        * package_version-revision_arch.deb

## Import Upstream


    1. First, create the first version of a package, outside of Git. Once it done, you can import the package using the import-dsc command.

~~~shell
    $ gbp import-dsc /path/to/package_0.1-1.dsc
~~~

    2. This will give output of its progress and make a few commits as well as have some new files and directories.

~~~shell
    $ ls
    package/
    package_0.1-1.orig.tar.gz
~~~~

    3. git-buildpackage command

    * Imported the package into the upstream branch
    * This has then been tagged upstream/0.1, which represent package's number
    * Imported debian/directory into master and upstream's files
    * Tagged the last commit as debian/0.1-1 as it finished package.

When there is a new version upstream, you could use the following steps to update to the latest version of upstream version.

    $ gbp import-orig -uscan
    $ gbp import-orig /path/to/new-upstream.tar.gz -u 0.2

*if the upstream tarball is already exist in the form of packagename_version.orgi.tar.gz, then the -u option is not required.


## Choosing your program

The steps to follow after choosing a package to create and in order to check if the package is already in the distribution archive are; the aptitude command, the debian packages web page and the debian package tracker web page.  Debian already has packages in the Debian archive which is much larger than that of contributors with upload rights.  If you are able to adopt the package, get the sources something like apt-get source but if you are installing new packages in Debian follow the steps below:

    1. First step is to confirm that the program works. 
    2. Second, Make sure no one is working on the package. If no one is working on it, file an ITP bug report to the wnpp pseudo-package using report bug. If someone else is already working on it, contact them if you feel you need to. 
    3. Third, the software must have a license. 
    4. Fourth, the program should not introduce security and maintenance concerns into the Debian system.  The program should be well documented, should not run setuid root and should not be a daemon.

As a new maintainer, you are encouraged to create simple packages, intermediate complexity packages and high complexity packages. Packaging a high complexity packages requires more knowledge than it takes to build a simple or intermediate packages. 

## Get the program, and try it out.

There are a few steps mentioned on this section to get the package and try it out. 

    1. First step is to find and download the original source code. 
    2. Second, if your package program comes in tar+gzip format with the extension .tar.gz, format with the extension .tar .bz2 or tar + xz format , it usually contain a directory called package-version with all the sources inside. 
    3. Third, if your program comes as some other sort of archive, you should unpack it with the appropriate tools and repack it. 

## Simple build systems

    1. Simple programs comes with a Makefile which can be compiled by invoking make. 
    2. Compile and run to make sure your program works so it wonot break something else while its installing or running. 
    3. You can run make clean to clean up the build directory. 
    4. You can use the make uninstall to remove all the installed files.

## Popular Portable Build Systems

Free software is often written in C or C++, and these often use portable build systems to make them portable across platforms. *Autotools* is one example of such a build system; it comprises *Autoconf, Automake, Libtool, and Gettext*. (Cmake is an alternative build system.)

With these build tools, they must be used to generate the Makefile and other required source files first. The program can then be built using *make; make install*.

The Autotools workflow generally proceeds as:

    1. Upstream runs autoreconf -i -f in the source directory and distributes the generated files along with the source.
    2. The user obtains the distributed source and runs ./configure && make in the source directory to compile the program in an executable source binary.

## Linux Distributions

### Ubuntu and Debian: How is Ubuntu related to Linux and Debian?

Ubuntu is a Linux distro that starts with the breadth of Debian and adds regular releases (every six months) commitment to security updated with nine months of support for every release. 

Ubuntu builds on the foundations of Debian's architecture and infrastructure but there are important differences such as, Ubuntu has its own user interface, a separate developer community and different release process. 
Debian is the rock upon which Ubuntu is built. It is a volunteer project that has developed and maintained a GNV/Linux Operating System of the same name over a decade.
Since its launch, the Debian project has grown to comprise more than 1,000 members with official developers status, alongside many more volunteers and contributors.
Debian is a free operating system. An operating system is the set of basic programs and utilities that make your computer run. At the core of operating system is a Kernel. The Kernel is the most fundamental program on the computer and does all the basic housekeeping and lets you start other programs. Debian system use Linux Kernel or the BSD Kernel. Today Debian encompasses over 20,000 packages of free open source applications and documentation.

**About Ubuntu**

Ubuntu is an Open-source project that develops and maintains a cross-platform, open-source operating system based on Debian. Upgrades are released every six months and support is granted by Canonical for up to five years. Canonical also provides commercial support for Ubuntu deployments across the desktop, the server and the cloud.

**How does Debian and Ubuntu fit together?** 

Ubuntu is proud to be based on Debian. Ubuntu developers want to maintain a healthy and collaborative relationship with Debian developers. Ubuntu has brought new users and developers to the GNU/Linux community

**How does Ubuntu differ from Debian?**

Ubuntu has a narrow focus--community based while Debian is Universal.

Ubuntu has its own community government structure somehow inspired by Debian's but different. (MOTU) The masters of the universe were team formed to look after the creation of the universe component.

**Debian for Ubuntu developers**

Ubuntu benefits from a strong Debian and Debian benefits from a strong Ubuntu.

Every Debian developer is also an Ubuntu developer because one way to contribute is to contribute to Debian 
Ubuntu is Debian based.

*Edubuntu*

A complete Linux based operating System targeted for primary and secondary education. It is freely available with community based support.
The Edubuntu community is built on the ideas enshrined in the Edubuntu manifesto: that software, especially for education should be available free of charge and that software tools should be usable by people in their local language and despite any disabilities. [14]

*Kubuntu*

An official derivative of Ubuntu Linux using ICDE instead of the GNOME or unity interfaces used by default in Ubuntu.

### RedHat/Fedora Based

### ArchLinux Based

## Linux Window Managers

### GNOME

GNOME (GNU Network Object Model Environment, pronounced as NOHM) is a graphical user interface (GUI) and set of computer Desktop applications for users of the Linux computer operating system. Its intended to make a Linux operating system easy to use for non-programmers and generally corresponds to the windows desktop interface and its most common applications. (Wikipedia)

### Englightnment 

**AbantOS** uses the Enlightenment Window Manager

## Elive background/history
	
## Elive purpose/objectives

## Teaching Linux Objectives

## Teaching  Linux Scope

## Finding Help

```shell
>>> help
```

# Installation of Elive

## Downloading Elive from [EliveCD.org](www.elivecd.org)

* The latest release of Elive can be downloaded from www.elivecd.org. The latest Beta release is the recommended version to download.
* Navigate to the Beta downloads by clicking Download, then Beta.
* For writing Elive to a USB drive, click the button labeled Elive Beta - USB. This is an Elive image specifically for writing to USBs. A window will open asking if you want to open or save the .img file. Click Save File and your browser will download the file.
* When the download finishes, open the folder to which it was saved and take note of the file's location.

## Release notes at [EliveCD.org](http://www.elivecd.org/news/releases/beta/)

The archive of release notes on the Beta releases of Elive can be found at http://www.elivecd.org/news/releases/beta/. Each entry contains many details on what features of Elive have been updated and improved in each release.

## Writing Elive to USB

* The recommended USB drive for writing and using Elive is the SanDisk Extreme USB 3.0 32 GB flash drive.
* If you are using Windows, you can write the Elive image to the USB using an image writer application. Elive suggests UsbWriter; other options include the application from Etcher.io or Win32DiskImager.
* With any application, first plug your USB drive into an open USB port on your computer. Using the application, select the Elive image from the location you downloaded it to, then select the correct USB drive and write the image.
* The written image will overwrite any other data contained on the USB drive. In some cases, the USB drive may need to be reformatted first before being able to write a new USB image (for instance, if a previous USB image was written to the drive). You can reformat the drive by right-clicking the drive in the Computer window or using Windows Disk Management.

## Booting Elive from the USB

* Power on your computer with the USB with Elive written to it plugged in. When the initial boot screen of your computer appears, hold down the Boot Menu key to load the boot menu. On many computers this is the F12 key, or one of the other F row keys. A list of common boot menu keys for various manufacturers can be found at http://www.elivecd.org/ufaqs/how-to-tell-the-computer-to-boot-from-another-device/.
* When the boot menu key appears, highlight the USB drive option and press Enter.
* After Elive initiates, the first Elive menu asking the method of installation will appear. For running Elive from the USB, select Live sessions with Persistence. This method will allow you to run Elive direct from the USB without installing the system on your separate hardware, and Persistence will save your configurations of Elive from session to session.

### What is BIOS?

First things first. BIOS stands for Basic Input Output System referred to as BIOS, is software stored on a small memory chip on the motherboard. It give the computer a set of instructions on how to perform some of the basic functions such as booting and keyboard control as well as configure the hardware in a computer including the hard drive, floppy drive, USB, optical drive, CPU, memory to list a few.

### What is EFI and UEFI? 

### What is a Master Boot Record?

### What is a GUID partition type? 

### Booting (Mac) from an external USB device

**References**
"[How to start up Mac from bootable media](http://www.idownloadblog.com/2015/09/14/how-to-start-up-mac-from-bootable-media/)"

### Booting (PC) from an external USB device

### USB Booting Issues

## Installing on hardware (or Live sessions w/ Persistence)

## Help with Installation

## Keeping Elive Up to Date

```shell    
>>> apug

```

```shell
>>> sudo apt-get dist-upgrade

```

# Graphics and Themes

Elive utilizes the Enlightenment Window Manager. Enlightenment utilizes a set of core libraries called (EFL), created specifically for Enlightenment. EFL is being recognized for its forward thinking approaches as users are wanting more from their operating systems UI. 

## Elive Tools

[elive-tools](https://github.com/Elive/elive-tools) are a set of handy and useful tools by Elive, for specialty use in the [Elive project]()

* audio-configurator
    - configure individual alsa cards
* bkp (save states)
* elivepaste (pastebin)
* waitfor
    - waits on the shell for a process that terminates and exits, its pretty useful for do things like: $ waitfor rebuild-packages && rebuild-iso
* user-manager
* make-cookies-with-chocolate


## Enlightenment Foundation Linux (EFL) Components

1. **Evas** 
    - Core scene graph and rendering
2. **Elina** 
    - Data strucures and low level helpers
3. **Edje** 
    - UI layout & animation data files for themes
4. **Eet** 
    - Data (de)serialization and storage
5. **Ecore** 
    - Core loop and system abstractions like x11
6. **Efreet** 
    - Freedesktop.org standards handling
7. **Eldbus** 
    - D-Bus glue and handling 
8. **Embryo** 
    - Tiny VM and compiler based on pawn
9. **Eeze** 
    - Device enumeration and access library
10. **Emotion** 
    - Video decode wrapping, glue and abstraction
11. **Ethumb** 
    - Thumbnailing handler
12. **Ephysics** 
    - Physics (bullet) wrapper and Evas glue
13. **EIO** 
    - Asynchronus I/O handling
14. **Evas Generic Loaders** 
    - Extra image loaders for complex image types
15. **Emotion Generic Players** 
    - Extra video decoders (for vlc)
16. **Elementary** 
    - Widgets and high level abstractions

## Customizing Your Elive/Enlightenment Theme

Elive currently has three theme options available by default.


1. **Default Theme**
2. **Elive Light Theme** 
    - A light theme
3. **Lucax Theme** 
    - A dark theme

### Change Themes

Right click on the desktop to bring up the menu, hover over Enlightenment to bring up enlightenment sub menu. Hover over about theme and click it, this will show you the current theme.  Close the about theme and right click on the desktop again to bring up the menu. Hover over Settings, scroll down and hover over Theme, you will see the three options available. Right click on the Theme you want and wait for it to change, this may take a minute or two depending on the theme. Open windows and applications will lock up during the transition, this is normal.


### Change Wallpaper

Right click on the desktop to bring up the menu. Hover over Settings and right click it. A settings window will appear. Right click on Wallpaper, the wallpaper settings will appear. Choose one the the wallpapers and click apply. 

### Live / Animated Wallpapers

There are two methods for doing this that I know of, here are both.


#### Live Wallpaper: Method 1


The first method is to download an HD Wallpaper in .gif format. You can download one here: Animated Wallpaper Sample gif. After you finish downloading your animated wallpaper you have to put it in the right directory open a Terminology terminal and navigate to /Downloads, or wherever you have your .gif wallpaper you downloaded. Now you have to move the wallpaper to the /usr/share/enlightenment/data/backgrounds/ . The command to move the file is:

~~~shell
sudo mv "name of file without quotes.gif" /usr/share/enlightenment/data/backgrounds/
~~~

Close the terminal and right click on the desktop to bring up the menu, click settings and then Wallpaper. The wallpaper settings will appear and it will open the directory where you just moved your wallpaper. Choose your animated wallpaper and click apply. Close the wallpaper settings.

NOTE: If you didn't see your wallpaper then your command may have been mistyped, don't forget to remove the quotes.


#### Live Wallpaper: Method 2


The second method is to download a HD wallpaper .gif and transfer it with the Thunar File Manager. Download your gif using the link above or any you choose in .gif format. Open the thunar file manager and from the home directory click downloads, or navigate to your .gif wallpaper. Open another thunar file manager window by clicking the icon in the dock again. Press (control) and (h) at the same time to show hidden files. Click .e and then e17, now click backgrounds. Drag and drop your animated wallpaper .gif from downloads to the backgrounds directory you just opened. After you transfer the wallpaper press (control) and (h) again to hide the config files. Close the file managers

Right click on the desktop to bring up the menu, click settings and then Wallpaper. The wallpaper settings will appear. In the top left click Personal, now you should see the wallpaper you transferred in the list below, click it. Now click apply and close the wallpaper settings.

## Advanced Customizations

### Settings/Look

#### Application Theme

#### Theme

#### Colors

#### Fonts

#### Borders

#### Ecomorph

#### Transitions

#### Scaling

#### Startup

### Settings/Apps

#### Personal Application Launchers

#### Favorite Applications

#### IBar Applications

#### Screen Lock Applications

#### Screen Unlock Applications

#### Restart Applications

#### Startup Applications

#### Default Applications

#### Desktop Environments


### Settings/Screen

#### Virtual Desktops

#### Screen Setup

#### Screen Lock

#### Blanking

#### Backlight


### Settings/Windows

#### Window Display

#### Window Focus

#### Window Geometry

#### Window List Menu

#### Window Remembers

#### Window Process Management

#### Windows Switcher

### Settings/Menus

#### Menu Settings


## Elive Desktop Features

### Main Elive Menus

### Finding installed applications

### Changing appearances/preferences

### Restoring settings


## Elive Desktop Help Features


# Installing/Removing applications

## Installing from the terminal (apt-get)

## Installing with a software package manager (i.e. Synaptic)

## Removing from the terminal

## Removing with a package manager

## Updating system and applications

## Help with Installation and Removal of Applications


# Command references

**cat _files_** - Prints files to screen.

**cd _directory_** - Change to particular directory.

**pwd** - Prints the current working directory.

**cp _files_ _dest_** - Copies files and directories.

**echo _string_** - Echoes a string to screen.

**ls _files_** - List files.

**mkdir _directory-name_** - Create directories.

**mv _file1 _file2_** - Move/rename files.

**rm _files_** - Remove files.

**grep _searchstring_ _files_** - Find a particular search-string in files.

**ps _options_** - Show current processes.

**kill _[-9]_ _PID_** - Send signal to terminate a particular process (process ID).

**su - _[username]_** - Switch user (e.g., become root).

**sudo _commannd_** - Execute a partcular command as root.

**dpkg -l _names_** - List packages.

**dpkg -I _pkg_.deb** - Show package information.

**dpkg -c _pkg_.deb** - List contents of package file.

**touch _file_** - Create an empty file.

**less _file_** - Display file content one screenful at at time.

**ln -s _target_ _linkname_** - Create a symbolic link from a target to linkname.

**lsof** - List open files and processes using them.

**find _directory_ -name _file_ -print** - Find a file location in a directory.

**gunzip _file_.gz** or **gzip _file_** - Uncompress or compress a .gz file.

**tar cvf _archive_.tar _file1_ _file2_ ...** - Create a compressed archive containing multiple files.

**tar xvf _archive_.tar** - Unpack a .tar file.

## Whatever (other) commands the class elects would be useful to include in a quick reference

## Where to get help

**man _page_ or man** - Read online help for a particular, or every, command.

**_command_ --help** - Brief help for a particular command.

## From basic to advanced

# Repository 

## Creating Repositories

## Setup and Maintain 

There are various reasons for building a repository yourself. You might just have a few packages with local modifications that you want to make available to apt, you may want to run a local mirror with those packages used by several machines to save bandwidth, or you build packages yourself and want to test them prior to publication.

## Anatomy of a Repo

A Debian repository contains several releases. Debian releases are named after characters from the "Toy Story" movies (wheezy, jessie, stretch, ...). The codenames have aliases, so called suites (stable, oldstable, testing, unstable). A release is divided in several components. In Debian these are named main, contrib, and non-free and indicate the licensing terms of the software they contain. A release also has packages for various architectures (amd64, i386, mips, powerpc, s390x, ...) as well as sources and architecture independent packages.

* Root Directory = dists 
* Each release subdirectory contains a cryptographically signed Release file and a directory for each component. 

## Repository Index File 

1. dpkg-scanpackages

~~~shell
$ dpkg-scansources dists/local/custom/source | gzip > dists/local/custom/source/Sources.gz
~~~

2. dpkg-scansources
~~~shell
$ dpkg-scanpackages dists/local/custom/binary-i586 | gzip > dists/local/custom/binary-i586/Packages.gz
~~~

## Override File

An override file can be optionally specified. If no override file exists/, dev/null must be provided as an explicit argument. 

## Working with Repos

Working with repositories may mean either of two different things:

1. You can use a repository with the apt family of programs (apt, apt-get, apt-cache, aptitude) to browse or install packages

~~~shell
echo "deb http://ftp.debian.org/debian stable main contrib non-free" >> /etc/apt/sources.list && apt-get update
~~~

2. You can set up a repository yourself and add, remove or replace packages in it.


## Repository References

    wiki.debian.org/DebianRepository
    wiki.debian.org/DebianRepository/Setup



# Elive Linux Basic System Administration


### By Mohamed Mohamud and Mohamed Mohamed

## Topic Areas

* Boot Process and Troubleshooting.
* Basic System Configuration.
* System Logging.
* Disk Management and File System Creation.
* Linux Networking and Configuration.
* User and Group Management
* Process and Job Management.
* Creating and Editing Text Files.
* Installing and Updating Software Packages.
* Scheduling and Automating Future Tasks.
* Service/Server Configuration (DHCP, FTP, DNS, SSH and HTTP).

#### Section 1.1 Boot Process

When you first boot an Elive Linux Operating System or any Linux OS it will go through the following processes to boot into the command line or desktop. When you press the power button on a machine that is running Elive, the first program that runs in the BIOS. BIOS stands for Basic Input Output System. We must clarify about the BIOS before we go any further.  To be clear the BIOS is not part of the Linux kernel. The BIOS is separate of the operating system, it is not part of the OS. It can be found on computers running other operating systems such as Windows and others. The BIOS is a software that is built into a computer's ROM (Read Only Memory) at the time of manufacturing the motherboard. The BIOS goes through two main stages to boot a Linux kernel. It first does POST (Power On Self Test). In this first stage the BIOS checks to ensure that peripheral devices are intact and ready to operate the system after boot up. Peripheral devices are things like the keyboard, monitor, mouse, RAM, CPU and so on. Below is a screenshot of a BIOS.

![bios](https://user-images.githubusercontent.com/26585912/33965743-b410efb4-e022-11e7-998d-6ea313aadbe0.PNG)

*Figure 1.1 BIOS.*


The next stage of the BIOS is that it looks for an MBR (Master Boot Record) on all the drives that are connected to the system. The BIOS has a boot menu which lists the boot order of all bootable devices that are available on a given system. Most BIOS by default are configured to check for the MBR on the first disk drive onboard. If there is no disk drive it'll check for a CD/DVD ROM or USB; which basically means it'll go through all the drives that are connected or onboard until it finds a drive with an MBR. Then it hands of the boot process to the drive that has the MBR. Another screenshot of the BIOS's boot menu.


![bios2](https://user-images.githubusercontent.com/26585912/33965751-bab72dc4-e022-11e7-9e55-7d56624d57c3.PNG)

*Figure 1.2 BIOS boot menu.*



The MBR is the first sector of a disk drive; it holds a table of partitions on the disk drive. The MBR will look all the partitions on the drive to find one that has a boot loader. There are many common boot loaders but Elive linux uses GRUB two. Below is a screenshot of the GRUB two startup menu that is getting ready to boot up an Elive linux. 

![grub2](https://user-images.githubusercontent.com/26585912/33965774-c7be93e0-e022-11e7-9b6e-3989717e64c3.png)
*Figure 1.3 GRUB Boot Menu.*

#### Section 1.2 Troublshoot Boot Process

Sometimes during the boot process you can into a some issues such as hearing a beep and the system getting stuck on first boot page.  Usually you get an error message that gives you a hint as to what might be the problem or an error that tells you exactly what may the problem. If you hear a beep that is usually in indication that the BIOS is either not detecting a peripheral or it is not able to communicate with it. In this case, follow the error message and figure out what is wrong with that device.  For instance, if you have an error that points to keyboard not detected make sure all the cabling to the PC is properly plugged in and that you don't have any lose connections. BIOS errors are not a kernel issue, that is just physical hardware issues. 

The next type of error you can run into is GRUB not finding an OS, this is a common error, this just means that when the BIOS handed off the boot process to the MBR which found GRUB, that specific disk drive does not have an OS installed on it or the installation is corrupt, the easiest solution is to reinstall the OS. But below are a few commands you can run to detect if you have a corrupt installation or of something else is going on. 

From the GRUB initial Boot Menu, use the up/down arrow keys to interrupt the boot process, then press "e" to enter into the GRUB configuration mode.

I have two linux OSs setup in dual boot but only one of them boots, the other one is not showing up in GRUB's menu. This is what to do when the system first boots. From the GRUB menu, enter "c" to go into grub command line configuration. Below is a screenshot of how that page looks like. Refer to the bottom of Figure 1.3 above to see the options.

![grub2-2](https://user-images.githubusercontent.com/26585912/33965787-d0580d88-e022-11e7-8c7a-6173b153ec52.png)
*Figure 1.4 Grub CLI*

In this menu, some of the available commands are the "ls" command and its options as well. In the above case, it is a simple problem, the second OS which is Debian 7 doesn't have a boot option declared in the grub file. By adding that configuration in grub that will take care of the problem and next time you reboot, GRUB will list multiple OS to boot from; in our case ELive linux and Debian 7. 


The command to change the system's hostname is **hostname** followed by whatever name you want to give it. You must be root to change the hostname or use the "sudo" option before the command. Running the **hostname** command by itself will return the current name of the system. 

```
root ~ >>> hostname
Elive
```
To change it now do the following. 

```  
root ~ >>> hostname Debian
root ~ >>> hostname
Elive
```
Now, we changed the name but when we run the **hostname** command again it returns the old name. The reason is because this change is temporary on this shell. You must start another shell or login again to see the change.  Besides that, most changes on the shell will not survive a system reboot, therefore, If you want to change the system's hostname permanently you must modify the /etc/hostname file.

```
[aali@elive:~]$ sudo nano /etc/hostname
```
![hostname](https://user-images.githubusercontent.com/26585912/33965802-de5fe694-e022-11e7-89dc-b75f741aaf91.PNG)
*Figure 2.1 /etc/hostname*

As you can see below when I run the command to check my environment PATH I don't have **/sbin** as part of the path. 

```
[aali@elive:~]$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
[aali@elive:~]$ 

```
So I can either temporarily add that directory to my PATH, or use the absolute path or use **sudo**. We'll try all three options below. 

Now I'm going to add it to my environment PATH so I can use it without **sudo** or without the absolute path. 

```
[aali@Elive:~]$
[aali@Elive:~]$ export PATH=$PATH:/sbin
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ ifconfig
eth0      Link encap:Ethernet  HWaddr 08:00:27:12:d8:f3
          inet addr:10.7.11.2  Bcast:10.7.11.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe12:d8f3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:223 errors:0 dropped:0 overruns:0 frame:0
          TX packets:180 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:26175 (25.5 KiB)  TX bytes:25322 (24.7 KiB)
eth1      Link encap:Ethernet  HWaddr 08:00:27:01:b6:23
          inet addr:10.0.2.6  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe01:b623/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:1397 errors:0 dropped:0 overruns:0 frame:0
          TX packets:820 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:1617497 (1.5 MiB)  TX bytes:93959 (91.7 KiB)

```

Now I'll try it with the absolute path. 

```

[aali@elive:~]$ /sbin/ifconfig eth0
eth0      Link encap:Ethernet  HWaddr 08:00:27:12:d8:f3  
          inet addr:10.7.11.2  Bcast:10.7.11.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe12:d8f3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:2683 errors:0 dropped:0 overruns:0 frame:0
          TX packets:2235 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:302465 (295.3 KiB)  TX bytes:369886 (361.2 KiB)

[aali@elive:~]$ 

          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:203 errors:0 dropped:0 overruns:0 frame:0
          TX packets:203 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:16729 (16.3 KiB)  TX bytes:16729 (16.3 KiB)

[aali@elive:~]$ 

```
As we see the interface did get an IP address from a dhcp server because I didn't configure it with an IP address manually and as we'll see the IP address configuration file will also confirm that. Servers usually need a static IP, since their IP address and hostname is usually configured on a DNS Server, they can't have their IP addresses changing every so often. Even though a DHCP entry can be configured to be static, usually servers have their IP addresses configured manually therefore, that is what we'll do right now. In order to do this we have to modify the **/etc/network/interfaces** for Elive linux. Let's first cat the file and see what it's default configuration looks like. 

```
[aali@elive:~]$ cat /etc/network/interfaces 
auto lo
iface lo inet loopback
iface eth0 inet dhcp

```

Now we're going to go in with nano or vi and modify that default configuration. All we need to do is change the dhcp option to static and define three things; an IP address, subnet mask and a default gateway. 

![interface](https://user-images.githubusercontent.com/26585912/33965817-ec71d648-e022-11e7-98de-92c4549d8a4e.PNG)
*Figure 2.2 Interface Configuration.*

Here is what it looks like after the changes. At this point we have configured a static IP on this machine, now all we need to do is save the file and reboot the machine. 

![interface2](https://user-images.githubusercontent.com/26585912/33965831-f724f3ea-e022-11e7-8e5f-86df4488be1b.PNG)
*Figure 2.3 Interface Configuration.*

```
alias apug='sudo apt-get update ; sudo apt-get upgrade'
[aali@elive:~]$ 
[aali@elive:~]$ 
```
Now we're going to run the **apug** alias to update our system to the latest available updates. 

```
[aali@elive:~]$ apug
[sudo] password for aali: 
Get:1 http://security.debian.org wheezy/updates Release.gpg [1,601 B]
Get:2 http://security.debian.org wheezy/updates Release [39.0 kB]                             
Hit http://repository.elivecd.org wheezy Release.gpg                  
Ign http://repository.elivecd.org wheezy/tests Translation-en
Fetched 798 kB in 13s (57.5 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages have been kept back:
  e17-conf e17-menus
The following packages will be upgraded:
  deliver e17-stable e17-stable-data e17-stable-dbg e17-stable-dev e17-theme-elive-light
  eliveremote evince evince-common firefox-esr icedove iceweasel libevdocument3-4
  libevview3-3 libxcursor-dev libxcursor1 thunderbird
The following packages will be DOWNGRADED:
  elive-tools
17 upgraded, 0 newly installed, 1 downgraded, 0 to remove and 2 not upgraded.
Need to get 127 MB of archives.
After this operation, 1,145 kB of additional disk space will be used.
Do you want to continue [Y/n]? 

```
Just answer "yes" when prompted and your system is updated to the latest Elive linux. Give it a quick reboot and you have an updated and patched system until next updates are released. 

#### Section 3.1 System Logging.

After building an Elive linux server updating and patching it, the next task you must do is configure the server to log errors and events before deployment.  System administrators rely on system monitoring tools to keep track of errors and other events that take place on the servers. This is needed for many reasons but, there are two main reasons why this information must be logged. The main reason is troubleshooting purposes, you need to see what errors a system is reporting and when the errors started in order to effectively troubleshoot and fix the problem. The second main reason is for security purposes.  If a system gets compromised system logs are very useful for getting more information on what happened and when. Because of the need for something that always monitors the linux machines, most linux distributions come with system logging tools that save errors and events to either remote servers or locally on the system. In this section we'll demonstrate how to use the most common syslog service that most distributions have available by default (rsyslog).  Our Elive developers packaged this in the main system installation like many linux distributions, so we don't even have to install it. All we need to do is configure and start the service. The rsyslog service saves all system logs in the **/var/log** directory. It logs messages in specific directories within the **/var/log*** directory for log messages such as mail, security, boot and other general system logs. 

In case you don't already have rsyslod installed here is the command to install it. 

```
[aali@Debian:~]$ sudo apt-get install rsyslog
[sudo] password for aali: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
rsyslog is already the newest version.
0 upgraded, 0 newly installed, 0 to remove and 2 not upgraded.
[aali@Debian:~]$ 

```

Now that we have installed the rsyslog service, this is how to start it. 

```
[aali@Debian:~]$ sudo service rsyslog start
Starting enhanced syslogd: rsyslogd.
[aali@Debian:~]$ 

```
Here is an example of rsyslog output. I'm going to change my session to root and we're going to look at what rsyslog does. 
```
[aali@Debian:/]$ su -
Password: 
                     Type 'help' to know the ton of Elive features available...
root ~ >>> 
root ~ >>> 
root ~ >>> tail -4 /var/log/auth.log
Dec 12 09:25:48 Debian sudo: pam_unix(sudo:session): session closed for user root
Dec 12 09:27:15 Debian su[14601]: Successful su for root by aali
Dec 12 09:27:15 Debian su[14601]: + /dev/pts/2 aali:root
Dec 12 09:27:15 Debian su[14601]: pam_unix(su:session): session opened for user root by aali(uid=1000)
root ~ >>> 

```
As you can see from the **tail -4 /var/log/auth.log** output which is getting the last 4 lines of that file, lines 2,3 and 4 shows me changing from my user account to the root account successfully. It logged all of that information. This was a specific area of rsyslog. We looked specifically at the log that is responbile for user account authentication **/etc/log/auth.log**. For more general log messages, use the **/var/log/messages** file which logs error messages, boot process errors and other general messages. Below is an output of the **/var/log/messages** file. 

```
root ~ >>> tail /var/log/messages
Dec 11 19:18:13 localhost kernel: [11947.623793] efreet_icon_cac[31030]: segfault at bf7c8ffc ip b7769a68 sp bf7c9000 error 6 in libeina.so.1.13.3[b7755000+62000]
Dec 11 19:18:14 localhost kernel: [11948.872127] efreet_icon_cac[31045]: segfault at bf36effc ip b76ecaa8 sp bf36f000 error 6 in libeina.so.1.13.3[b76d8000+62000]
Dec 11 19:18:16 localhost kernel: [11951.017977] efreet_icon_cac[31070]: segfault at bf4a6fec ip b76b064b sp bf4a6ff0 error 6 in libeina.so.1.13.3[b769a000+62000]
Dec 11 19:18:18 localhost kernel: [11952.629468] efreet_icon_cac[31241]: segfault at bf5c2ffc ip b76e964b sp bf5c3000 error 6 in libeina.so.1.13.3[b76d3000+62000]
Dec 12 08:47:24 Debian kernel: imklog 5.8.11, log source = /proc/kmsg started.
Dec 12 08:47:24 Debian rsyslogd: [origin software="rsyslogd" swVersion="5.8.11" x-pid="9726" x-info="http://www.rsyslog.com"] start
Dec 12 09:16:53 Debian kernel: [55068.459661] e1000: eth0 NIC Link is Down
Dec 12 09:16:59 Debian kernel: [55074.473418] e1000: eth0 NIC Link is Up 1000 Mbps Full Duplex, Flow Control: RX
Dec 12 09:19:59 Debian kernel: imklog 5.8.11, log source = /proc/kmsg started.
Dec 12 09:19:59 Debian rsyslogd: [origin software="rsyslogd" swVersion="5.8.11" x-pid="13768" x-info="http://www.rsyslog.com"] start
```

For more specific logs, please refer to the respective file for each specific log you need, such as mail, kernel, and cron information or errors etc...
To customize the rsyslog service to your own needs please refer to the man pages for **rsyslog.conf**.

#### Section 4 Disk Management.

There are many tools available for disk management for most linux distributions. Elive comes prepackaged with fdisk, parted and gparted for GUI. The gparted tool is easy to use and self explanatory if you have a desktop GUI configuration. In this section will cover how to use both gparted since Elive is a GUI configuration of Debain and we'll also demonstrate fdisk for command line configuration for those that may prefer the CLI option.  Were going to start with the command line in this section. 

#### Section 4.1 Fdisk.

The fdisk tool commands are in the **/sbin/** directory like the **ifconfig** command. So all the **fdisk** commands we run here must be run with **sudo** or the full path **/sbin/fdisk**.  

```
[aali@Debian:/]$ sudo fdisk -l

Disk /dev/sda: 32.2 GB, 32212254720 bytes
255 heads, 63 sectors/track, 3916 cylinders, total 62914560 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000e629f

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1           16384     3088383     1536000   82  Linux swap / Solaris
/dev/sda2   *     3088384    62914559    29913088   83  Linux
[aali@Debian:/]$ 

```
The **fdisk -l** option displays the currently configured disk drives in the system. I have added another partition but it is not configured yet with a file system so fdisk doesn't display it until it is configured with a file system and formatted. Below is the **ls -l /dev/sd*** command that shows us indeed we have another unformatted and unpartitioned disk drive in this sytem. 

```
root ~ >>> ls -l /dev/sd*
brw-rw---T 1 root disk 8,  0 Dec 11 11:12 /dev/sda
brw-rw---T 1 root disk 8,  1 Dec 11 11:12 /dev/sda1
brw-rw---T 1 root disk 8,  2 Dec 11 11:12 /dev/sda2
brw-rw---T 1 root disk 8, 16 Dec 11 11:12 /dev/sdb

```
Since most commands I'm running in this section require **sudo**, I'll just switch to the root account and work from that account instead of having to use **sudo** over and over again. With that said, now I'm going to configured our new partition with **fdisk**. Go in with **fdisk /dev/sdb** which sdb is the second drive in this machines. Once we're in fdisk configuration mode, we have a lot of options, we can run the **m** option to have all the available commands/options listed for us. 

```
[aali@Debian:/]$ sudo fdisk /dev/sdb
[sudo] password for aali: 
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0xeeb6ce10.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): 

```


```
Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

```
The p option prints the current partition table. We don't have any on our current partition so when we run the p action below is what we get. 

```
root ~ >>> 
Command (m for help): p

Disk /dev/sdb: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xccd1e794

   Device Boot      Start         End      Blocks   Id  System

Command (m for help):
Command (m for help): p

Disk /dev/sdb: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xccd1e794

   Device Boot      Start         End      Blocks   Id  System

```
Now we're going to use the **n** action to format and create partitions within this disk drive. 

```
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): prim        ^H^H
Partition number (1-4, default 1): 
Using default value 1
First sector (63-41943039, default 63): 
Using default value 63
Last sector, +sectors or +size{K,M,G} (63-41943039, default 41943039): 
Using default value 41943039

Command (m for help):

```
So we created a new primary partition in our second disk drive. Running the **p** action again will confirm that. 

```
Command (m for help): p

Disk /dev/sdb: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xccd1e794

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1              63    41943039    20971488+  83  Linux

Command (m for help):

```
Now we're going to save out configuration changes by using the **w** action, without using the **w** action, the changes we just made will not get written on the disk drive. 

```
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.

```
Now the next step is to configure this disk with a file system. We'll use the linux ext4 file system, because I want to install another linux distribution on this disk drive and setup a dual boot. We'll use the **mkfs** command to make a file system on this drive. 

```

root ~ >>> mkfs -t ext4 /dev/sdb1                                                                         1
mke2fs 1.42.5 (29-Jul-2012)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
1310720 inodes, 5242872 blocks
262143 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=0
160 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
        4096000

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done  

```
So our new disk drive is now formatted with a primary partion and a ext4 file system. We're going to mount it to see if our changes worked. I will create a directory called OpenSuse in the **/mnt/** directory and mount our new partion on there. 

```
root ~ >>> mkdir /mnt/OpenSuse
root ~ >>> mount /dev/sdb1 /mnt/OpenSuse   
root ~ >>> df -H
Filesystem                                             1K-blocks    Used Available Use% Mounted on
rootfs                                                  29912168 7864028  22048140  27% /
udev                                                       10240       0     10240   0% /dev
tmpfs                                                     414900     820    414080   1% /run
/dev/disk/by-uuid/43cbad91-9996-48b1-9c5b-7429a2513b4b  29912168 7864028  22048140  27% /
tmpfs                                                       5120       0      5120   0% /run/lock
tmpfs                                                    1136980    9724   1127256   1% /run/shm
/dev/sdb1                                               20511324   44992  19401376   1% /mnt/OpenSuse
root ~ >>> 

```
So as we can see from the commands above, our file was successfully mounted to the new **/mnt/OpenSuse** directory. We also run the **df -h** command to see the side and available space.  The mount point we have is only good while the system is up and running, it doesn't survive a reboot of the system. If we want a partition to survive a reboot, we add the mount point configuraton to the **/etc/fstab** and that is what we'll do next.  

``` 
root ~ >>> nano /etc/fstab
  GNU nano 2.2.6                       File: /etc/fstab                                                     

# Generated by the Elive installer

UUID=43cbad91-9996-48b1-9c5b-7429a2513b4b    /    reiserfs    defaults,commit=300,relatime,notail    0    1
UUID=eeb92a85-e4da-4f9c-8fae-52b0c77c851f  none        swap        sw        0    0
UUID=e70e3a87-88d4-4dea-af37-e140bd8db0d9  /mnt/OpenSuse ext4  defaults=rw,dev,exec,suid             0    1

```

Now that we have the disk added to the **/etc/fstab**, I rebooted the system to test it and it is still mounted after the reboot. 

#### Section 5 Elive Networking and Configuration.

An Elive linux machine must get internet access in order to get the latest bug fixes, security patches and upgrades. In this section we'll cover how to configure your system to get on the network and how to troubleshoot any network issues. We have already discussed how to setup a network interface for DHCP or how to manually configure it statically. Here we'll start with bringing up and interface and shutting it down. 

Usually interfaces comeup by default when a system is first booted up. But, we will start off with bringing up the interface or shutting it down manually. 

We start of with only one physical interface (eth1) and the loopback interface. 

```
[aali@Elive:~]$ /sbin/ifconfig
eth1      Link encap:Ethernet  HWaddr 08:00:27:01:b6:23
          inet addr:10.0.2.6  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe01:b623/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:212 errors:0 dropped:0 overruns:0 frame:0
          TX packets:296 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:25131 (24.5 KiB)  TX bytes:38125 (37.2 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:101 errors:0 dropped:0 overruns:0 frame:0
          TX packets:101 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:8508 (8.3 KiB)  TX bytes:8508 (8.3 KiB)

[aali@Elive:~]$
[aali@Elive:~]$ clear
[aali@Elive:~]$

```
Now we're going to bring up the eth0 with the **ifup** command. 

```
[aali@Elive:~]$ sudo ifup eth0
Internet Systems Consortium DHCP Client 4.2.2
Copyright 2004-2011 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/eth0/08:00:27:12:d8:f3
Sending on   LPF/eth0/08:00:27:12:d8:f3
Sending on   Socket/fallback
DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval 8
DHCPREQUEST on eth0 to 255.255.255.255 port 67
DHCPOFFER from 10.67.181.254
DHCPACK from 10.67.181.254
Reloading /etc/samba/smb.conf: smbd only.
bound to 10.67.181.2 -- renewal in 1572 seconds.
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ sudo ifconfig
eth0      Link encap:Ethernet  HWaddr 08:00:27:12:d8:f3
          inet addr:10.67.181.2  Bcast:10.67.181.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe12:d8f3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:238 errors:0 dropped:0 overruns:0 frame:0
          TX packets:34 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:39320 (38.3 KiB)  TX bytes:4213 (4.1 KiB)

eth1      Link encap:Ethernet  HWaddr 08:00:27:01:b6:23
          inet addr:10.0.2.6  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe01:b623/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:547 errors:0 dropped:0 overruns:0 frame:0
          TX packets:569 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:55408 (54.1 KiB)  TX bytes:75352 (73.5 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:115 errors:0 dropped:0 overruns:0 frame:0
          TX packets:115 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:9980 (9.7 KiB)  TX bytes:9980 (9.7 KiB)

[aali@Elive:~]$
[aali@Elive:~]$

```
As you can see above, we've got three interfaces up and running. The loopback which is virtual interface, eth0 which we just brought up and interface eth1. Now we're going to take **eth0** back down. 

```
[aali@Elive:~]$ sudo ifdown eth0
[sudo] password for aali:
Internet Systems Consortium DHCP Client 4.2.2
Copyright 2004-2011 Internet Systems Consortium.
All rights reserved.
For info, please visit https://www.isc.org/software/dhcp/

Listening on LPF/eth0/08:00:27:12:d8:f3
Sending on   LPF/eth0/08:00:27:12:d8:f3
Sending on   Socket/fallback
DHCPRELEASE on eth0 to 10.82.64.70 port 67
Reloading /etc/samba/smb.conf: smbd only.
[aali@Elive:~]$
[aali@Elive:~]$ sudo ifconfig
eth1      Link encap:Ethernet  HWaddr 08:00:27:01:b6:23
          inet addr:10.0.2.6  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe01:b623/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:641 errors:0 dropped:0 overruns:0 frame:0
          TX packets:658 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:63624 (62.1 KiB)  TX bytes:93540 (91.3 KiB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:129 errors:0 dropped:0 overruns:0 frame:0
          TX packets:129 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:11449 (11.1 KiB)  TX bytes:11449 (11.1 KiB)

[aali@Elive:~]$

```
Now we're back to the first two interfaces that were up.  It is as simple as that to bring up and down interfaces in Elive linux. 

#### Section 5.2 Network Connectivity.

In this section we'll test a remote Elive system to see if it is up or down by using ping and traceroute. 

##### Note: There are no firewalls in this network or any ACLs (Access Control Lists) blocking anything. In a typical network infrastructure, routers, switches or firewall ACLs could typically drop ping or traceroute packets. 

```
C:\Users\IEUser>ping 10.7.11.2

Pinging 10.7.18.2 with 32 bytes of data:
Reply from 10.7.11.171: Destination host unreachable.
Reply from 10.7.11.171: Destination host unreachable.
Reply from 10.7.11.171: Destination host unreachable.
Reply from 10.7.11.171: Destination host unreachable.

Ping statistics for 10.7.11.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),

C:\Users\IEUser>
C:\Users\IEUser>

```
Since we can't ping the remote Elive host and we know there aren't any security devices filtering our ICMP packets. We'll do a traceroute to see where the packets might be dropping. 

```
C:\Users\IEUser>tracert 10.7.11.2

Tracing route to 10.7.11.2 over a maximum of 30 hops

  1    <1 ms    <1 ms    <1 ms  10.0.2.1
  2    gw.testlab.home [10.7.11.171]  reports: Destination host unreachable.

Trace complete.

C:\Users\IEUser>

```

Traceroute shows that another device, likely the gateway of the Elive host is dropping the packets. This could mean that the Elive host is down or its interface that we're trying to hit is down. In this scenario the Elive host is up, but I intentionally shutdown the interface for this demo. So our traceroute is stopping at the ELive's network gateway. I'm going to bring back up the interface and we'll experiment the behavour and compare it with when it's down. 

```

C:\Users\IEUser>ping 10.7.11.2

Pinging 10.67.181.2 with 32 bytes of data:
Reply from 10.7.11.2: bytes=32 time<1ms TTL=63
Reply from 10.7.11.2: bytes=32 time=1ms TTL=63
Reply from 10.7.11.2: bytes=32 time=1ms TTL=63
Reply from 10.7.11.2: bytes=32 time=1ms TTL=63

Ping statistics for 10.7.11.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

C:\Users\IEUser>
C:\Users\IEUser>tracert 10.7.11.2

Tracing route to elive.testlab.home [10.7.11.2]
over a maximum of 30 hops:

  1    <1 ms    <1 ms     2 ms  10.0.2.1
  2     1 ms     1 ms     1 ms  elive.testlab.home [10.7.11.2]

Trace complete.

C:\Users\IEUser>

```

The result is almost the same except we're getting ping replies which indicates that the remote Elive host we're pinging is up. In this case we didn't even need to do a traceroute but we and the traceroute completes until it gets to the elive host. We can configure an interface IP statically from the command line as well but this configuration will not survive a reboot. Below is the syntax.

**sudo ifconfig eth0 10.7.11.2 netmask 255.255.255.0 up**

We can also configure a gateway for this as follows. 

**sudo route add default gw 10.7.11.171**

Again, most CLI configurations don't survive a reboot. If you want the configurations to be persistent, you must do it in their respective files in the **/etc** directory. 

We can check to see our routing table with the following command. 


```
[aali@Elive:~]$ sudo route
[sudo] password for aali:
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         10.0.2.1        0.0.0.0         UG    0      0        0 eth1
10.0.2.0        *               255.255.255.0   U     0      0        0 eth1
10.7.11.0       *               255.255.255.0   U     0      0        0 eth0
[aali@Elive:~]$

```
We can check to see what ports are open on the system with the **netstat** command. 


```
[aali@Elive:~]$ netstat
Active Internet connections (w/o servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0     64 10.0.2.6:ssh            10.0.2.7:50219          ESTABLISHED
Active UNIX domain sockets (w/o servers)
Proto RefCnt Flags       Type       State         I-Node   Path
unix  22     [ ]         DGRAM                    11579    /dev/log
unix  2      [ ]         DGRAM                    11581    /var/spool/postfix/dev/log
unix  3      [ ]         STREAM     CONNECTED     14528    /var/run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     16569    @/tmp/dbus-aD6ZVUtxkT
unix  3      [ ]         STREAM     CONNECTED     15670    @/tmp/.X11-unix/X0
unix  3      [ ]         STREAM     CONNECTED     14540    /var/run/dbus/system_bus_socket
unix  3      [ ]         STREAM     CONNECTED     523650
unix  3      [ ]         STREAM     CONNECTED     13326    /var/run/acpid.socket
unix  3      [ ]         STREAM     CONNECTED     16547    @/tmp/.X11-unix/X0
unix  3      [ ]         STREAM     CONNECTED     12127
unix  2      [ ]         DGRAM                    15452
unix  3      [ ]         STREAM     CONNECTED     16463

```

#### Section 6 User and Group Management.

It is very simple to add users in Elive Linux, there are a couple of commands available to do that. Since Elive is Debian based the favored command for Debian based distrubtions is the **adduser** command. We also have the **useradd** command which is popular on Fedora/Redhat based flavors. Both commands are available on Elive but we'll sticket with the **adduser** command. The way the **adduser** command works is that it is a shell script that calls the **useradd** command in the background and runs it in the background. Where as when using the **useradd** you have to tell it specifically what you need with the command options. 

```
root ~ >>> adduser jdoe
Adding user `jdoe' ...
Adding new group `jdoe' (1004) ...
Adding new user `jdoe' (1004) with group `jdoe' ...
Creating home directory `/home/jdoe' ...
Copying files from `/etc/skel' ...
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
Changing the user information for jdoe
Enter the new value, or press ENTER for the default
        Full Name []: John Doe
        Room Number []: 3270
        Work Phone []: 651-200-2000
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
You have new mail.
root ~ >>>

```
By using the **adduser** plus the username you want to use, the system prompts you for whatever options you want to give that username. Now it created a user account, a home directory, it assigned the user a shell and a password all with one command. If we were to use the **useradd** command it would do the same except we would have to use options and specify exactly what we need it, in way it is more flexible but at the sametime it could be more typing. 

As we can see below, jdoe now has a home directory with his username, most users are given a home directory based on their username by default. 

```
root ~ >>> ls -l /home                                                                                                      1
total 9
drwxr-xr-x 53 aali   aali   2800 Dec 13 07:33 aali
drwxr-xr-x 27 asmith asmith 1560 Dec 11 16:44 asmith
drwxr-xr-x 27 jdoe   jdoe   1528 Dec 13 08:02 jdoe
drwxr-xr-x 27 momo   momo   1528 Dec 11 16:41 momo
drwxr-xr-x 27 sfarsi sfarsi 1584 Dec  3 12:16 sfarsi
root ~ >>>

```
By default users are assigned the bash shell, and we can confirm that by checking the **/etc/passwd** file. 

```
[aali@Elive:~]$ cat /etc/passwd | grep jdoe
jdoe:x:1004:1004:John Doe,3270,651-200-2000,:/home/jdoe:/bin/bash
[aali@Elive:~]$
[aali@Elive:~]$

```
We will now create a group and add jdoe to that group. This is again straight foward. We will use the groupadd command to do this. 

```
[aali@Elive:~]$
[aali@Elive:~]$ cat /etc/group | grep AbantOS
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ sudo groupadd AbantOS
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ cat /etc/group | grep AbantOS
AbantOS:x:1006:
[aali@Elive:~]$

```

So we checked the group name to see if it's already in use, it wasn't then we created the group called "AbantOS". Now we're going to add jdoe to that group. 

By running the **id** plus the username we'll verify to see which group jdoe belongs to then we'll add him to the AbantOS group. 

```

[aali@Elive:~]$ id jdoe
uid=1004(jdoe) gid=1004(jdoe) groups=1004(jdoe)
[aali@Elive:~]$

```

It shows jdoe is currently part of his main group (1004). Now we'll add him to the AbantOS group. 
```

[aali@Elive:~]$ sudo usermod -aG AbantOS jdoe
[sudo] password for aali:
[aali@Elive:~]$
[aali@Elive:~]$ id jdoe
uid=1004(jdoe) gid=1004(jdoe) groups=1004(jdoe),1006(AbantOS)
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ cat /etc/group | grep jdoe
jdoe:x:1004:
AbantOS:x:1006:jdoe
[aali@Elive:~]$

```

We added jdoe to the AbantOS group by using the **usermod** command. Then we varified our work by running the **id** command again and we checked the group file configuration (/etc/group) to make sure he was indeed added. 

To delete a user account is very simple, all we need to do is use the userdel command with the -r option. If we don't use the -r option the user's directory won't get deleted. So we have to make sure and add the -r option as follows. 

```
[aali@Elive:~]$ sudo userdel asmith
[sudo] password for aali:
[aali@Elive:~]$ ls -l /home
total 9
drwxr-xr-x 53 aali   aali   2800 Dec 13 07:33 aali
drwxr-xr-x 27   1003   1003 1560 Dec 11 16:44 asmith
drwxr-xr-x 27 jdoe   jdoe   1528 Dec 13 08:02 jdoe
drwxr-xr-x 27 momo   momo   1528 Dec 11 16:41 momo
drwxr-xr-x 27 sfarsi sfarsi 1584 Dec  3 12:16 sfarsi
[aali@Elive:~]$
[aali@Elive:~]$ sudo userdel jdoe
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ ls -l /home
total 9
drwxr-xr-x 53 aali   aali   2800 Dec 13 07:33 aali
drwxr-xr-x 27   1003   1003 1560 Dec 11 16:44 asmith
drwxr-xr-x 27   1004   1004 1528 Dec 13 08:02 jdoe
drwxr-xr-x 27 momo   momo   1528 Dec 11 16:41 momo
drwxr-xr-x 27 sfarsi sfarsi 1584 Dec  3 12:16 sfarsi
[aali@Elive:~]$
[aali@Elive:~]$ sudo userdel -r sfarsi
userdel: sfarsi mail spool (/var/mail/sfarsi) not found
[aali@Elive:~]$
[aali@Elive:~]$ ls -l /home
total 7
drwxr-xr-x 53 aali aali 2800 Dec 13 07:33 aali
drwxr-xr-x 27 1003 1003 1560 Dec 11 16:44 asmith
drwxr-xr-x 27 1004 1004 1528 Dec 13 08:02 jdoe
drwxr-xr-x 27 momo momo 1528 Dec 11 16:41 momo
[aali@Elive:~]$

```

As we see above, I deleted two user accounts (asmith and jdoe) but their home directories didn't get deleted. Then I deleted the sfarsi account with the -r option and the home directory got deleted as well. 

Now we're going to delete a group to demo how that works too. 

```
[aali@Elive:~]$ cat /etc/group | grep LinuxThree
LinuxThree:x:1005:
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ man groupadd
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ sudo groupdel LinuxThree
[sudo] password for aali:
[aali@Elive:~]$
[aali@Elive:~]$ cat /etc/group | grep LinuxThree
[aali@Elive:~]$
[aali@Elive:~]$

```

So above we demonstarted how to deleted the a group with the **groupdel** command and then verify by checking the group configuration file in **/etc/group**. 

User passwords are held in the /etc/shadow file which is encrypted. If using the **useradd** command. One can use the **passwd** command to have an encrypted password created for the user and saved in **/etc/shadow** file. 

To modify existing user's account expiration dates, password renewal and account closure date you can use the **chage** command. 

Below we can see all the available parameters with that command. 


```

[aali@Elive:~]$ sudo chage -l momo
[sudo] password for aali:
Last password change                                    : Dec 12, 2017
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[aali@Elive:~]$

```

For instance, if we want to change the default setting for account expires "never" to a specific date, we can run the command as follows. 

```
[aali@Elive:~]$ sudo chage -E 03/2/2018 momo
[sudo] password for aali:
Sorry, try again.
[sudo] password for aali:
[aali@Elive:~]$
[aali@Elive:~]$ sudo chage -l momo
Last password change                                    : Dec 12, 2017
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : Mar 02, 2018
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
[aali@Elive:~]$

```

There, we changed the user momo's account expiration date from never to 3/2/2018. 

#### Section 7 Process and Job Management.

There are several ways to manage processes in Elive Linux. We'll concentrate on a couple of commands for this section. We can run the **top** command or **ps** command to see running processes.  

```

[aali@Elive:~]$ top
top - 09:09:45 up  1:40,  1 user,  load average: 0.09, 0.23, 0.30
Tasks: 146 total,   2 running, 144 sleeping,   0 stopped,   0 zombie
%Cpu(s):  4.4 us,  2.0 sy,  1.2 ni, 92.0 id,  0.5 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   4148964 total,   873744 used,  3275220 free,    42272 buffers
KiB Swap:  1535996 total,        0 used,  1535996 free,   538212 cached

  PID USER      PR  NI  VIRT  RES  SHR S  %CPU %MEM    TIME+  COMMAND
 3571 root      20   0 98040  43m  23m R   6.0  1.1   6:44.12 Xorg
 4921 aali      23   3  159m  47m  21m S   6.0  1.2   5:13.68 terminology
 4675 aali      20   0  234m 150m  51m S   4.6  3.7   4:47.48 enlightenment
    7 root      20   0     0    0    0 S   0.3  0.0   0:49.49 rcu_sched
   13 root      20   0     0    0    0 S   0.3  0.0   0:08.12 ksoftirqd/1
 6098 aali      23   3  9912 3704 3044 S   0.3  0.1   0:00.63 sshd
19284 aali      23   3  5088 2556 2152 R   0.3  0.1   0:00.06 top
    1 root      20   0  2308 1520 1412 S   0.0  0.0   0:01.35 init
    2 root      20   0     0    0    0 S   0.0  0.0   0:00.00 kthreadd
    3 root      20   0     0    0    0 S   0.0  0.0   0:08.31 ksoftirqd/0
    5 root       0 -20     0    0    0 S   0.0  0.0   0:00.00 kworker/0:0H

```
Next we see the process running with the **ps -aux** command. 

```
[aali@Elive:~]$ ps -aux
warning: bad ps syntax, perhaps a bogus '-'?
See http://gitorious.org/procps/procps/blobs/master/Documentation/FAQ
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0   2308  1520 ?        Ss   07:29   0:01 init [2]
root         2  0.0  0.0      0     0 ?        S    07:29   0:00 [kthreadd]
root         3  0.1  0.0      0     0 ?        S    07:29   0:08 [ksoftirqd/0]
root         5  0.0  0.0      0     0 ?        S<   07:29   0:00 [kworker/0:0H]
root         7  0.8  0.0      0     0 ?        S    07:29   0:50 [rcu_sched]
root       202  0.0  0.0      0     0 ?        S<   07:29   0:00 [scsi_tmf_2]
root       219  0.0  0.0      0     0 ?        S<   07:29   0:00 [kworker/0:1H]
root       222  0.0  0.0      0     0 ?        S<   07:29   0:00 [kworker/1:1H]
root       254  0.0  0.0      0     0 ?        S<   07:29   0:00 [md]
root       269  0.0  0.0      0     0 ?        S<   07:29   0:00 [bioset]
root       276  0.0  0.0      0     0 ?        S<   07:29   0:00 [reiserfs/sda2]
root       431  0.0  0.0   3100  2580 ?        Ss   07:29   0:00 udevd --daemon
root       581  0.0  0.0   3096  2352 ?        S    07:29   0:00 udevd --daemon
root       588  0.0  0.0      0     0 ?        S<   07:29   0:00 [iprt]
root       599  0.0  0.0      0     0 ?        S<   07:29   0:00 [kpsmoused]
root      2043  0.0  0.0   2416  1948 ?        Ss   07:29   0:00 /sbin/rpcbind -w
statd     2077  0.0  0.0   2672  2356 ?        Ss   07:29   0:00 /sbin/rpc.statd
root      2082  0.0  0.0      0     0 ?        S<   07:29   0:00 [rpciod]
root      2084  0.0  0.0      0     0 ?        S<   07:29   0:00 [nfsiod]
root      2092  0.0  0.0   2604   184 ?        Ss   07:29   0:00 /usr/sbin/rpc.idmapd
root      2561  0.0  0.0  28016  2588 ?        Sl   07:29   0:00 /usr/sbin/rsyslogd -c5
root      2688  0.0  0.0   1728    56 ?        Ss   07:29   0:00 /usr/sbin/acpi_fakekeyd
root      2930  0.0  0.0   2036  1648 ?        Ss   07:32   0:00 /usr/sbin/acpid
root      3156  0.0  0.0   3520  1788 ?        Ss   07:32   0:00 /usr/sbin/irqbalance
root      3229  0.0  0.1   5964  4356 ?        Ss   07:32   0:00 /usr/sbin/apache2 -k start
www-data  3232  0.0  0.0   5752  3392 ?        S    07:32   0:00 /usr/sbin/apache2 -k start
www-data  3235  0.0  0.0 227372  3984 ?        Sl   07:32   0:00 /usr/sbin/apache2 -k start
www-data  3236  0.0  0.0 227380  3984 ?        Sl   07:32   0:00 /usr/sbin/apache2 -k start
root      3374  0.0  0.0   4508  2496 ?        Ss   07:32   0:00 /usr/sbin/bluetoothd
root      3394  0.0  0.1  32664  5596 ?        Sl   07:32   0:00 /usr/sbin/lightdm
root      3418  0.0  0.2  27924  8492 ?        Ssl  07:32   0:00 /usr/sbin/NetworkManager
root      3472  0.0  0.0   3096  2288 ?        S    07:32   0:00 udevd --daemon
root      3484  0.0  0.0      0     0 ?        S<   07:32   0:00 [krfcommd]
root      3569  0.0  0.0   2080  1716 ?        Ss   07:32   0:00 /usr/sbin/inetd
root      3644  0.0  0.1   7032  4728 ?        S    07:32   0:00 /usr/sbin/modem-manager
root      3749  0.0  0.1  15740  5100 ?        Sl   07:32   0:00 /usr/lib/urfkill/urfkilld
root      3857  0.0  0.0      0     0 ?        S<   07:32   0:00 [iprt]
root      3875  0.0  0.0  23608  2552 ?        Sl   07:32   0:03 /usr/sbin/VBoxService
root      3961  0.0  0.0   4416  2032 ?        Ss   07:32   0:00 /usr/sbin/cron
root      4031  0.0  0.0   2776  2164 ?        S    07:32   0:00 dovecot/log
root      4033  0.0  0.0   4564  3940 ?        S    07:32   0:00 dovecot/config
root      4076  0.0  0.1  15788  5916 ?        Sl   07:32   0:00 lightdm --session-child 12 19
root      4310  0.0  0.0   4556  2100 ?        S    07:32   0:00 /usr/sbin/vsftpd
root      4340  0.0  0.0  10256  4136 ?        Ss   07:32   0:00 /usr/sbin/nmbd -D
root      4366  0.0  0.1  19884  8236 ?        Ss   07:32   0:00 /usr/sbin/smbd -D
root      4390  0.0  0.0  20400  3564 ?        S    07:32   0:00 /usr/sbin/smbd -D

```

Processes use a few command commands, such as kill, ps, jobs fg and bg. We'll demo how to use these commands.  


#### Section 8 Creating and Editing Text Files. 

In Elive Linux we have several tools we can use to create and edit text files.  First of all, we can create and edit files straight from the command line itself. Secondly, we have text editors such as vi, nano, emacs etc...

We can create a file with the **touch** command. All this does is create an empty file. If you use the command on an existing file it just changes the date and timestamp of the file.  Here is an example of touch. 


```
[aali@Elive:~]$ ls -l
total 4
-rw-r--r-- 1 aali aali 202 Dec 11 06:41 about.html
drwxr-xr-x 9 aali aali 320 Nov  5 17:51 Documents
drwxr-xr-x 4 aali aali 416 Nov  6 08:18 Downloads
drwxr-xr-x 2 aali aali 112 Dec  2 20:55 fontconfig
drwxr-xr-x 2 aali aali 208 Nov  5 17:51 Music
drwxr-xr-x 5 aali aali 296 Nov  5 17:51 Pictures
drwxr-xr-x 3 aali aali 136 Dec 11 06:33 projects
drwxr-xr-x 2 aali aali 120 Nov  5 17:51 Public
-rw-r--r-- 1 aali aali   0 Nov  5 17:54 testFile.txt
drwxr-xr-x 4 aali aali 144 Nov  5 17:51 Videos
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ touch file1
[aali@Elive:~]$ ls -l
total 4
-rw-r--r-- 1 aali aali 202 Dec 11 06:41 about.html
drwxr-xr-x 9 aali aali 320 Nov  5 17:51 Documents
drwxr-xr-x 4 aali aali 416 Nov  6 08:18 Downloads
-rw-r--r-- 1 aali aali   0 Dec 13 09:24 file1
drwxr-xr-x 2 aali aali 112 Dec  2 20:55 fontconfig
drwxr-xr-x 2 aali aali 208 Nov  5 17:51 Music
drwxr-xr-x 5 aali aali 296 Nov  5 17:51 Pictures
drwxr-xr-x 3 aali aali 136 Dec 11 06:33 projects
drwxr-xr-x 2 aali aali 120 Nov  5 17:51 Public
-rw-r--r-- 1 aali aali   0 Nov  5 17:54 testFile.txt
drwxr-xr-x 4 aali aali 144 Nov  5 17:51 Videos
[aali@Elive:~]$
[aali@Elive:~]$ ls -l file1
-rw-r--r-- 1 aali aali 0 Dec 13 09:24 file1
[aali@Elive:~]$
[aali@Elive:~]$ touch file1
[aali@Elive:~]$ ls -l file1
-rw-r--r-- 1 aali aali 0 Dec 13 09:25 file1
[aali@Elive:~]$

```
First we checked to see what files we have, then we created a file called file1 with the **touch** command. Then we updated the timestamp on the file by running it with the **touch** command again.


```

[aali@Elive:~]$ echo "This text is going to file one." > file1
[aali@Elive:~]$
[aali@Elive:~]$ cat file1
This text is going to file one.
[aali@Elive:~]$
[aali@Elive:~]$ echo "We're adding a second line by editing file1" >> file1
[aali@Elive:~]$
[aali@Elive:~]$ cat file1
This text is going to file one.
We're adding a second line by editing file1
[aali@Elive:~]$
[aali@Elive:~]$

```

Now, we updated the file1 file and edited it again all from the command line just by using the **echo** command.  

We can also use the text editors such as vi or nano or emacs, Elive has them all available as part of the main distribution. 


```
[aali@Elive:~]$ vi file2
CSApprox skipped; terminal only has 8 colors, not 88/256
Try checking :help csapprox-terminal for workarounds
Press ENTER or type command to continue
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ ls -l | grep file
-rw-r--r-- 1 aali aali  76 Dec 13 09:27 file1
-rw-r--r-- 1 aali aali  63 Dec 13 09:30 file2
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ cat file1
This text is going to file one.
We're adding a second line by editing file1
[aali@Elive:~]$
[aali@Elive:~]$ cat file2
This text is in file two.
This text is being written with vi.
[aali@Elive:~]$
[aali@Elive:~]$

```

As you can see above, we've created a file called file2 with vi and added a couple of lines to it. So your options are many when it comes to creating and editing text files in Elive Linux. 


#### Section 9 Installing and Updating Packages. 

We have a few tools or commands available to install and update software packages in Elive Linxu. We have the **aptitude**, **apt-get**, **dpkg** and we have synaptic package manager when working on the GUI. We'll demonstrate some of these commands in the following steps. The **aptitude** command can be used to install new software packages, update existing ones or delete packages.  

```
[aali@Elive:~]$ sudo aptitude show vlc
E: Could not get lock /var/lib/dpkg/lock - open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?
Package: vlc
State: installed
Automatically installed: no
Version: 1:2.0.6-dmo3+deb7u2
Priority: optional
Section: video
Maintainer: Christian Marillat <marillat@deb-multimedia.org>
Architecture: i386
Uncompressed Size: 3,449 k
Depends: fonts-freefont-ttf, vlc-nox (= 1:2.0.6-dmo3+deb7u2), libaa1 (>= 1.4p5), libavcodec54 (>= 8:1.0.0), libavutil51 (>=
         8:1.0.0), libc6 (>= 2.8), libcaca0 (>= 0.99.beta17-1), libfreetype6 (>= 2.2.1), libfribidi0 (>= 0.19.2), libgcc1 (>=
         1:4.1.1), libgl1-mesa-glx | libgl1, libqtcore4 (>= 4:4.8.0), libqtgui4 (>= 4:4.7.0~beta1), libsdl-image1.2 (>= 1.2.10),
         libsdl1.2debian (>= 1.2.11), libstdc++6 (>= 4.6), libtar0, libva-x11-1 (> 1.0.15~), libva1 (> 1.0.15~), libvlccore5 (>=

         2.0.0), libx11-6, libxcb-composite0, libxcb-keysyms1 (>= 0.3.9), libxcb-randr0 (>= 1.1), libxcb-shm0, libxcb-xv0 (>= 1.2),
         libxcb1 (>= 1.6), libxext6, libxinerama1, libxpm4, zlib1g (>= 1:1.2.3.3)
Recommends: vlc-plugin-notify (= 1:2.0.6-dmo3+deb7u2), vlc-plugin-pulse (= 1:2.0.6-dmo3+deb7u2), xdg-utils
Suggests: videolan-doc
Breaks: vlc-data (< 1.1.5), vlc-nox (< 2.0.2)
Replaces: vlc-nox (<= 1:2.0.1-dmo3)
Provides: mp3-decoder
Description: multimedia player and streamer
 VLC is the VideoLAN project's media player. It plays MPEG, MPEG-2, MPEG-4, DivX, MOV, WMV, QuickTime, WebM, FLAC, MP3, Ogg/Vorbis
 files, DVDs, VCDs, podcasts, and multimedia streams from various network sources.

 VLC can also be used as a streaming server that duplicates the stream it reads and multicasts them through the network to other
 clients, or serves them through HTTP.

 VLC has support for on-the-fly transcoding of audio and video formats, either for broadcasting purposes or for movie format
 transformations. Support for most output methods is provided by this package, but features can be added by installing additional
 audio plugins (vlc-plugin-pulse, vlc-plugin-sdl) or video plugins (vlc-plugin-sdl).
Homepage: http://www.videolan.org/vlc/

[aali@Elive:~]$

```

The **aptitude show package_name** shows us a lot of information about a certain package. Above I ran the command with the vlc package and it shows that the package is installed and up to date, the maintainer, architecture dependents etc.. Next we'll check to see of a package that is not installed. And then install it. 

```

[aali@Elive:~]$ sudo aptitude show wireshark
Package: wireshark
State: not installed
Version: 1.12.1+g01b65bf-4+deb8u6~deb7u7
Priority: optional
Section: net
Maintainer: Balint Reczey <balint@balintreczey.hu>
Architecture: i386
Uncompressed Size: 2,474 k
Depends: libc6 (>= 2.7), libcairo2 (>= 1.2.4), libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.31.8), libgtk-3-0 (>= 3.3.16),
         libnl-3-200 (>= 3.2.7), libnl-genl-3-200 (>= 3.2.7), libnl-route-3-200, libpango1.0-0 (>= 1.14.0), libpcap0.8 (>= 0.9.8),
         libportaudio2 (>= 19+svn20101113), libwireshark5 (>= 1.12.0~rc3), libwiretap4 (>= 1.12.0~rc1), libwsutil4 (>= 1.12.0~rc3),
         zlib1g (>= 1:1.1.4), wireshark-common (= 1.12.1+g01b65bf-4+deb8u6~deb7u7), xdg-utils
Conflicts: ethereal (< 1.0.0-3)
Replaces: ethereal (< 1.0.0-3)
Description: network traffic analyzer - GTK+ version
 Wireshark is a network "sniffer" - a tool that captures and analyzes packets off the wire. Wireshark can decode too many protocols
 to list here.

 This package provides the GTK+ version of wireshark.
Homepage: http://www.wireshark.org/

[aali@Elive:~]$
[aali@Elive:~]$

```

It shows that wireshark is not installed and tell us everything we need to know about it from it's dependencies to it's current version and maintainer etc...All we need to do to install it is **aptitude install wireshark**

We'll use **apt-get install** to install the wireshark software package.  


```
[aali@Elive:~]$ sudo apt-get install wireshark
[sudo] password for aali:
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
  libc-ares2 libsmi2ldbl libwireshark-data libwireshark5 libwiretap4 libwsutil4 wireshark-common
Suggested packages:
  snmp-mibs-downloader wireshark-doc
The following NEW packages will be installed:
  libc-ares2 libsmi2ldbl libwireshark-data libwireshark5 libwiretap4 libwsutil4 wireshark wireshark-common
0 upgraded, 8 newly installed, 0 to remove and 2 not upgraded.
Need to get 16.3 MB of archives.
After this operation, 59.8 MB of additional disk space will be used.
Do you want to continue [Y/n]? y
Get:1 http://security.debian.org/debian-security/ wheezy/updates/main libc-ares2 i386 1.9.1-3+deb7u2 [75.2 kB]
Get:2 http://security.debian.org/debian-security/ wheezy/updates/main libwsutil4 i386 1.12.1+g01b65bf-4+deb8u6~deb7u7 [109 kB]
Get:3 http://security.debian.org/debian-security/ wheezy/updates/main libwiretap4 i386 1.12.1+g01b65bf-4+deb8u6~deb7u7 [224 kB]
Get:4 http://security.debian.org/debian-security/ wheezy/updates/main libwireshark-data all 1.12.1+g01b65bf-4+deb8u6~deb7u7 [1,151 kB]
Get:5 http://security.debian.org/debian-security/ wheezy/updates/main libwireshark5 i386 1.12.1+g01b65bf-4+deb8u6~deb7u7 [13.3 MB]
Get:6 http://security.debian.org/debian-security/ wheezy/updates/main wireshark-common i386 1.12.1+g01b65bf-4+deb8u6~deb7u7 [206 kB]
Get:7 http://security.debian.org/debian-security/ wheezy/updates/main wireshark i386 1.12.1+g01b65bf-4+deb8u6~deb7u7 [999 kB]
Get:8 http://deb.debian.org/debian/ wheezy/main libsmi2ldbl i386 0.4.8+dfsg2-7 [146 kB]
Fetched 16.3 MB in 5s (2,856 kB/s)
Preconfiguring packages ...
Selecting previously unselected package libsmi2ldbl:i386.
(Reading database ... 223536 files and directories currently installed.)
Unpacking libsmi2ldbl:i386 (from .../libsmi2ldbl_0.4.8+dfsg2-7_i386.deb) ...
Selecting previously unselected package libc-ares2:i386.
Unpacking libc-ares2:i386 (from .../libc-ares2_1.9.1-3+deb7u2_i386.deb) ...
Selecting previously unselected package libwsutil4:i386.
Unpacking libwsutil4:i386 (from .../libwsutil4_1.12.1+g01b65bf-4+deb8u6~deb7u7_i386.deb) ...
Selecting previously unselected package libwiretap4:i386.
Unpacking libwiretap4:i386 (from .../libwiretap4_1.12.1+g01b65bf-4+deb8u6~deb7u7_i386.deb) ...
Selecting previously unselected package libwireshark-data.
Unpacking libwireshark-data (from .../libwireshark-data_1.12.1+g01b65bf-4+deb8u6~deb7u7_all.deb) ...
Selecting previously unselected package libwireshark5:i386.
Unpacking libwireshark5:i386 (from .../libwireshark5_1.12.1+g01b65bf-4+deb8u6~deb7u7_i386.deb) ...
Selecting previously unselected package wireshark-common.
Unpacking wireshark-common (from .../wireshark-common_1.12.1+g01b65bf-4+deb8u6~deb7u7_i386.deb) ...
Selecting previously unselected package wireshark.
Unpacking wireshark (from .../wireshark_1.12.1+g01b65bf-4+deb8u6~deb7u7_i386.deb) ...
Processing triggers for man-db ...
Processing triggers for hicolor-icon-theme ...
Processing triggers for menu ...
Processing triggers for shared-mime-info ...
Processing triggers for desktop-file-utils ...
Setting up libsmi2ldbl:i386 (0.4.8+dfsg2-7) ...
Setting up libc-ares2:i386 (1.9.1-3+deb7u2) ...
Setting up libwsutil4:i386 (1.12.1+g01b65bf-4+deb8u6~deb7u7) ...
Setting up libwiretap4:i386 (1.12.1+g01b65bf-4+deb8u6~deb7u7) ...
Setting up libwireshark-data (1.12.1+g01b65bf-4+deb8u6~deb7u7) ...
Setting up libwireshark5:i386 (1.12.1+g01b65bf-4+deb8u6~deb7u7) ...
Setting up wireshark-common (1.12.1+g01b65bf-4+deb8u6~deb7u7) ...
Setting up wireshark (1.12.1+g01b65bf-4+deb8u6~deb7u7) ...
Processing triggers for menu ...
[aali@Elive:~]$
[aali@Elive:~]$

```
Now we're going to verify if we have wireshark installed with the  option and remove it with **aptitude**. 

```
[aali@Elive:~]$ sudo aptitude show wireshark
Package: wireshark
State: installed
Automatically installed: no
Version: 1.12.1+g01b65bf-4+deb8u6~deb7u7
Priority: optional
Section: net
Maintainer: Balint Reczey <balint@balintreczey.hu>
Architecture: i386
Uncompressed Size: 2,474 k
Depends: libc6 (>= 2.7), libcairo2 (>= 1.2.4), libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.31.8), libgtk-3-0 (>= 3.3.16),
         libnl-3-200 (>= 3.2.7), libnl-genl-3-200 (>= 3.2.7), libnl-route-3-200, libpango1.0-0 (>= 1.14.0), libpcap0.8 (>= 0.9.8),
         libportaudio2 (>= 19+svn20101113), libwireshark5 (>= 1.12.0~rc3), libwiretap4 (>= 1.12.0~rc1), libwsutil4 (>= 1.12.0~rc3),
         zlib1g (>= 1:1.1.4), wireshark-common (= 1.12.1+g01b65bf-4+deb8u6~deb7u7), xdg-utils
Conflicts: ethereal (< 1.0.0-3)
Replaces: ethereal (< 1.0.0-3)
Description: network traffic analyzer - GTK+ version
 Wireshark is a network "sniffer" - a tool that captures and analyzes packets off the wire. Wireshark can decode too many protocols
 to list here.

 This package provides the GTK+ version of wireshark.
Homepage: http://www.wireshark.org/

[aali@Elive:~]$

```

The **aptitude** command shows that wireshark is installed and we'll use it again to remove it. 

```
[aali@Elive:~]$ sudo aptitude remove wireshark
The following packages will be REMOVED:
  libc-ares2{u} libsmi2ldbl{u} libwireshark-data{u} libwireshark5{u} libwiretap4{u} libwsutil4{u} wireshark wireshark-common{u}
0 packages upgraded, 0 newly installed, 8 to remove and 2 not upgraded.
Need to get 0 B of archives. After unpacking 59.8 MB will be freed.
Do you want to continue? [Y/n/?] y
(Reading database ... 223853 files and directories currently installed.)
Removing wireshark ...
Removing wireshark-common ...
Removing libwireshark5:i386 ...
Removing libc-ares2:i386 ...
Removing libsmi2ldbl:i386 ...
Removing libwireshark-data ...
Removing libwiretap4:i386 ...
Removing libwsutil4:i386 ...
Processing triggers for desktop-file-utils ...
Processing triggers for hicolor-icon-theme ...
Processing triggers for shared-mime-info ...
Processing triggers for man-db ...
Processing triggers for menu ...

[aali@Elive:~]$

```

We can use the apt-get and aptitude command interchangebly.  We can also install packages with the .deb extension using the **dpkg** command. 


![desktop](https://user-images.githubusercontent.com/26585912/33965879-1ed03c2e-e023-11e7-94f4-0ec624b2f5c1.PNG)
*Figure 9.1 dpkg.*

The installation steps after running the command. 

![desktop2](https://user-images.githubusercontent.com/26585912/33965890-26af8c7e-e023-11e7-8827-e6104a066421.PNG)
*Figure 9.2 dpkg*

To remove run the **dpkg -r ** option for remove and the package is removed.


#### Section 10 Scheduling and Automating Tasks.

In Elive Linux we can use a couple of tools to automate and schedule tasks for a later date or time. We have a couple of tools available to most Linux distrubtions that are usually used for this tasks. There is a tool called "at" which used to schedule one time tasks and another tool called cron which is used for recurring taks. In this section we'll take a look at how to use these tools. 
The "at" tool is not installed on Elive linux by default but it is very easy to install. We can use any of the package installation tools that we've already discussed in the previous section. 

```

[aali@Elive:~]$ sudo aptitude install at
The following NEW packages will be installed:
  at
0 packages upgraded, 1 newly installed, 0 to remove and 2 not upgraded.
Need to get 43.4 kB of archives. After unpacking 178 kB will be used.
Get: 1 http://deb.debian.org/debian/ wheezy/main at i386 3.1.13-2+deb7u1 [43.4 kB]
Fetched 43.4 kB in 0s (78.6 kB/s)
Selecting previously unselected package at.
(Reading database ... 223536 files and directories currently installed.)
Unpacking at (from .../at_3.1.13-2+deb7u1_i386.deb) ...
Processing triggers for man-db ...
Setting up at (3.1.13-2+deb7u1) ...
[ ok ] Starting deferred execution scheduler: atd.

[aali@Elive:~]$

```

All we need to do is type the "at" command followed by the time or date we would like changes to take place. There are many options available for the command please refer to the man pages for the details. 

```
[aali@Elive:~]$ at now +10min
warning: commands will be executed using /bin/sh
at> sudo shutdown -r now
at> <EOT>
at> <EOT>
job 1 at Wed Dec 13 11:48:00 2017
[aali@Elive:~]$

```
What we did here is schedule a system reboot ten minutes from now. When you set up an "at" task for later, you end the job with <EOT> which stands for end of task, and you hit ctrl+d to save it. You can verify your at job with the atq command as follows. 

```
[aali@Elive:~]$ atq
1       Wed Dec 13 11:48:00 2017 a aali
[aali@Elive:~]$
[aali@Elive:~]$

```

If we're not sure about a task or we just want to cancel it we can use the **atrm** plus the task/job number as follows. 

```
[aali@Elive:~]$ atrm 1
[aali@Elive:~]$ atq

```
Next we'll look at "cron" which is used for recurring tasks. 
The cron tool is preinstalled in Elive Linux so we don't need to install it unless you have trouble using it. To start using cron run the make sure the service is up and running by using the **service cron status** command. 

```
[aali@Elive:~]$ sudo service cron status
[ ok ] cron is running.
[aali@Elive:~]$
[aali@Elive:~]$

```
Now we can run cron as follows. 

[aali@Elive:~]$ sudo crontab -e
[sudo] password for aali:

![crontab](https://user-images.githubusercontent.com/26585912/33965939-580385b4-e023-11e7-80ac-9947adff6581.PNG)
*Figure 10.1 crontab*

Once you're in cron configuration mode, you have the option of configuring a task to with the following five parameters. Minute, hour day of the month, month and day of the week then the command to run. In our case we configured our cron to run a system upgrade at 2am every Monday. 

```
0 2 * * 2 sudo apt-get upgrade -y

```

The first number is for the minutes which takes a value of 0-59, the second is for the hour which takes a value of 0-23, the * * are day of the month and month respectively. They take a value of 1-31 and 1-12 respectively. Then whatever command you would like to run. Please refer to the man pages for further configurations options for cron. 



#### Section 11 Service Configuration

In this section we'll setup our Elive Linux as an SSH, FTP, HTTP server. 

To setup ftp, install vsftpd and start the service. 

```
[aali@Elive:~]$ sudo aptitude install vsftpd
[sudo] password for aali:
No packages will be installed, upgraded, or removed.
0 packages upgraded, 0 newly installed, 0 to remove and 2 not upgraded.
Need to get 0 B of archives. After unpacking 0 B will be used.

```

Package is already installed, so we'll just start it. 

```

[aali@Elive:~]$ sudo service vsftpd restart
Stopping FTP server: vsftpd.
Starting FTP server: vsftpd.
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ sudo service vsftpd status
vsftpd is running
[aali@Elive:~]$
[aali@Elive:~]$

```
The vsftpd daemon has an anonymous account setup by default so we'll access that just for demo. 

![ftp](https://user-images.githubusercontent.com/26585912/33965943-62a488c4-e023-11e7-8210-473f2493e6e5.PNG)

*Figure 11.1 ftp.*

```
[aali@Elive:~]$ sudo service ssh status
[sudo] password for aali:
[ ok ] sshd is running.
[aali@Elive:~]$

```
Next we started ssh and logged it to the server with ssh from a remote windows machine. 

```
[aali@Elive:~]$ w
 13:32:26 up  6:03,  1 user,  load average: 0.18, 0.25, 0.30
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
aali     pts/2    win.testlab.home 07:34    3.00s  1.12s  0.00s w
[aali@Elive:~]$

```

Next we started our http/web server by first starting apache2 service. 

```
[aali@Elive:~]$ sudo service apache2 start
[....] Starting web server: apache2apache2: using 127.0.0.1 for ServerName
httpd (pid 29714) already running
. ok
[aali@Elive:~]$
[aali@Elive:~]$
[aali@Elive:~]$ sudo service apache2 status
Apache2 is running (pid 29714).
[aali@Elive:~]$

```

![apache2](https://user-images.githubusercontent.com/26585912/33965956-6f46aa26-e023-11e7-92f6-71d4c831af3b.PNG)
*Figure 11.2 apache.*


### System Logging commands

     a. dmesg (Will list the kernel boot process)

     b. To see the logs for apache go to /var/log/apache2
     
     3. Disk Management and File System

     a. After loading a new drive use "ls -l /dev/sd* or hd etc...

     b. To partiton the drive using fdisk use this command as root "fdisk /dev/sd*"

### Managing Users and Groups

    a. To modify an existing user account use "usermod " command

    b. To delete an existing account an all it's files use "userdel -r"

### Linux Networking

    a. To check the routing table use this command "route"

    b. To add a gateway for an interface use this command "route add default gw x.x.x.x inter_name"

### Processes and Job Management

    a. To kill an existing process use the "kill PID" command

    b. To view processes in a tree like format use "pstree | more"
    
    7.  Creating, Viewing and Editing files

    a. To use vi, use this command "vi fileName"

    b. To go into insert mode hit "i" or "a"

 ### Installing and Updating software packages

    a. If you want to see what packages you have installed use this "dpkg -l"

    b. To remove a package you don't need use "apt-get remove package_name"



 ### Network Storage and Management

    a. To mount an NFS show drive use "mount /dev/sdx /mnt/mount_point_name"

    b. To unmount an NFS drive use "umount /dev/sdx /mnt/mount_point_name"

### Service Configuration

     a. To see the status of a running service use this "service service_name status"

     b. To restart a running service use this "service service_name restart"

### Here is a simple shell script that will print something on the screen.

     vi fileName

     #!/bin/bash

     echo "Hello Linux Class!"

     exit file and run with " ./fileName"
     
    This is our work for week14, We've also created a branch on github and uploaded this file. 

1. Boot process and Troubleshooting
The command to get into grub after boot up is "grub"
And use "quit" to get out of grub

2. System Logging

3. Disk Management and File System Creation

To modify or add a new partition use "parted /dev/sdx" command
To display existing partitions within a disk use "print"

4. Managing Linux Users and Groups

to modify an existing group use "groupmod"
to display a specific user's group and uid use "id username"

5. Linux Networking (setting up networking and config)
"ip addr" is another command that display the info of interfaces
To display network connections use "netstat -r"

6. Process and Job Management

7. Creating, Viewing and Editing Text Files
To quit vi without saving your changes type escape ":q!"
To cut a like in a file using vi, use "y" from edit mode

8. Installing and Updating Software Packages
To compile a source file and install
./configure (from it's config direcotory where the configure file is)
make
make install

9. Scheduling and Automating Future tasks
crontab file is one of the tools used to auto make future tasks. 
to get into the crontab file use this "crontab -e"
to list any pending jobs use "crontab -l"

10. Network Storage and Management (NFS, SMB)

Here is a mount command for nfs
mount -o rw serverName:/dir /mnt/dir

11. Service Configuration (DHCP, FTP, DNS, SSH, HTTP etc...) 
to install ftp and start it use the following commands. 
sudo apt-get install vsftpd
sudo service vsftpd start

12. Linux Shell Scripting
     
   
   ### Command line: File permissions The commands for modifying file permissions and ownership are:

chmod – change permissions

chown – change ownership.

remember the only user that can actually modify the permissions or ownership of a file is either the current owner.
or the root user.

you can change the permissions of the folder to give them access. One way to do this would be to issue the command: 

-sudo chmod -R ugo+rw /DATA/SHARE

to break it down:

" sudo – this is used to gain admin rights for the command on any system that makes use of sudo (otherwise you'd have to 'su' to root and run the above command without 'sudo')

chmod – the command to modify permissions

-R – this modifies the permission of the parent folder and the child objects within

ugo+rw – this gives User, Group, and Other read and write access."

###   To get a blend of consents you include the numbers together. 

For instance to get read and execute consents the number you require is 5, to get read and compose authorizations the number is 6 and to get compose and execute consents the number is 3

Keep in mind you have to indicate 3 numbers as a major aspect of the chmod command. The first number is for the owner permissions, the second number is for the gathering authorizations and the last number is for every other person.

For instance to get full authorizations on the proprietor, read and execute consents on the gathering and no authorizations for any other individual sort the following= hmod 750 test 

If you wish to change the group name that owns a folder use the chgrp command.

if you don't have the correct permission to create a group you may need to use sudo to gain extra privileges or switch to an account with valid permissions using the su command.

Now you can change the group for a folder by typing= chgrp accounts <foldername>
 

!! ensure you work on doing this on working frameworks like ubuntu to show signs of improvement understanding.


## User Management

To maintain the privacy of users of the Linux system and manage their accounts and supervision, as well as track the problems they will go through, find the solutions to these problems, and inform users the changes, update, and upgrade the system. Even if there is a one user using the system it was always a challenge for the managing users. I will show you some tools and some tasks assigned to add the user to Linux system where you see adding and deleting accounts is an easy process for managing users.

Before we talk about how to creating and deleting a user account I will talk about the most important text file(/etc/passwd) because is processing the all most and necessary details and information about all users in the Linux system.

This file it is open to read only for users, and it is read and writable for root account.

### /etc/passwd file

The /etc/passwd file containe only one separate line, limited by a colon (:) for each user account in the system, also it is stored the information for each user using the Linux system. Every time we add new user to the system all details and information for the new user will stored in the same file. 

root /h/jamal  >>> sudo  cat  /etc/passwd

jamal: x:1001:1001: jamal Ibrahim, 1,,:/home/jamal :/bin/bash

tyler: x:1002:1002: tyler, 1,,:/home/tyler :/bin/bash

awil: x:1003:1003: awil, 1,,:/home/awil :/bin/bash

jacquie: x:1004:1004: jacquie, 1,,:/home/jacquie :/bin/bash

matthew: x:1005:1005: matthew, 1,,:/home/matthew :/bin/bash

to get depth in the file I will take the entry of my user name jamal.

 

Jamal       :    x      :   1001 :   1001   :    jamal Ibrahim   ,    1   ,,   :   /home/jamal    :   /bin/bash

{---1----}{---2---}{---3-----}{---4----}{---------------5---------------}{----------6------}{-------7-----}

### /etc/passwd fields

    1. **Username field**: This field show the user account name; this user name must be used every time when we log in to the system.
    2. **Password field**: Second field is the Password field, not showing the actual password though. The 'x' in this field show the password is encrypted and saved in the /etc/shadow file.
    3. **UID field**: Every time when a user account is created, it is assigned with a user id or UID (UID for the user 'jamal' is 1001, in this case) and this field specifies the same.
    4. **GID field**: Similar to the UID field, this field specifies which group the user belongs to, the group details being present in /etc/group file.
    5. **Comment/Description/User Info field**: This field is the short comment/description/information of the user account.
    6. **User Home Directory**: Every time the user sign in to the system, he is taken to his Home directory, where all his personal files reside. This field provides the absolute path to the user's home directory (/home/jamal in this case).
    7. **Shell**: This field show, the user has access to the shell mentioned in this field (user 'jamal' has been given access to /bin/bash or simply bash shell).

## Adding new user accounts to Linux system

# Package Management

Elive is based on Debian, the same distribution flavor as Ubuntu, so we use .deb files for our packages and those files collectively live in a repository.

## Debian Package Management

Debian Package Management details can be found in the [Debian Manuals](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html)

--------------------------------------------------------------------------------
package             popcon           size       description
---------           --------         ------     -------------
apt                 V:871, I:999     3647       Advanced Packaging Tool (APT), front-end for dpkg providing "http", "ftp", and "file" archive access methods (apt-get/apt-cache commands included)

aptitude            V:136, I:778     4415       interactive terminal-based package manager with aptitude(8)

tasksel             V:40, I:972      374        tool for selecting tasks for installation on the Debian system (front-end for APT)

unattended-upgrades V:188, I:364     254        enhancement package for APT to enable automatic installation of security upgrades

dselect             V:4, I:59        2498       terminal-based package manager (previous standard, front-end for APT and other old access methods)

dpkg                V:932, I:999     6745       package management system for Debian

synaptic            V:70, I:453      7793       graphical package manager (GNOME front-end for APT)

apt-utils           V:391, I:997     1103       APT utility programs: 

apt-listchanges     V:350, I:838     365        package change history notification tool

apt-listbugs        V:7, I:12        451        lists critical bugs before each APT installation

apt-file            V:13, I:77       82         APT package searching utility -- command-line interface

apt-rdepends        V:0, I:6         40         recursively lists package dependencies
--------------------------------------------------------------------------------
Table: Debian package management (Manual), List of Debian package management tools

Source: [Debian package management manual](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html) and [Popularity Contest Stats](https://qa.debian.org/popcon.php?package=apt)

## Elive Packages

In order to understand the components of the Elive repository here are a couple commands

### Listing Packages

```shell

for package in $( dpkg -l | awk '{print $2}' ) ; 
    do if apt-cache policy "$package" 2>/dev/null | grep -qs "repository.elivecd.org" ; 
    then echo -e "${package%:*} \tis in Elive repos" ; 
    fi ;  
done

```

### Listing Package Count

```shell

cat /var/lib/apt/lists/repository.elivecd.org_*Packages | grep "^Package:" | wc -l

```

## Installing Packages 

```shell

sudo apt-get

```

1. acptool: replacement of apmtool. For laptop users.

2.  afflib- dpg- on-disk format for storing complete forensic information: sudo apt-get install afflib-dbg, allows files to be digitally signed, chain-of-custody and long-term file integrity. Allows disk images containing privacy sensitive material to be stored on the Internet. Posix stricpting (Linux coding).

3. balsa-dbg: get sudo apt-get install balsa-dbg. Mail client from GNOME desktop. Supports both POP3 and IMAP.

4. cython3-dbg: sudo apt-get install cython-dbg. Libraries built against versions of Python configured with pydebug. Allows python configuration.

5. /etcldibbler/client.conf w/new version. DHCPv6 clients: sudo apt-get install dibbler-client

6. sudo apt-get install DVd+rw-tools-dbp

7. sudo apt-get update

8. sudo apt-get install build-essential save files and execute.

9. sudo apt-get install enigma 


# Testers & Quality Assurance

## What are some patterns we should test? 

## What steps should Testers & QA go through? 


# User Case Studies

## How are we going to use this? 

## What are your biggest challenges to learning Linux? 

## What tools and techniques are helpful to you? 

Talk to potential users!

# Credits

## Contributors

### Jacquie Kerongo

Project Lead and Troubleshooting
How is Ubuntu related to Linux and Debian?

### Angela Griffin

Package Management

### Jamal Ibrahim 

User Management

### Mohamed Mohamud

Repository & Systems Administration

### Mohamed Mohamed

folder creation and management | Systems Administration

### Tyler Cobb

Scribe
Quality Assurance / Testing

### Rodas Mekonnen

Cat Hearding
Editing

### Roxanne Thao

Package Management

### Doung Doth

USB Booting (Mac)

### Blia Vue

QA: Debian Package Management Workflow

## Editors

### Kautz, D.E.

Project Editor
Original Draft (2017-10-30)
Installation of Elive

### Harmon, M.J.

Formatting
