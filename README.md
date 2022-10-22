# Canon iP2600 drivers for Linux
Guide to install iP2600 drivers on linux

This guide will discuss debian based installation only. It should be possible on arch as well if you manage to add the libraries those binaries complain about.
The objective of this guide is to get an iP2600 printer to work on linux so it can be put on the LAN with CUPS.

## Patching (You can skip if you download the ones provided in the debian folder)
Because this driver is very old (Release date: 08 July 2009) the package **libcupsys2** is not available anywhere anymore, but the important bit is the lib file ```libcups.so.2``` which is available in the package **libcups2**. It is necessary to change this dependency so apt can find the library this driver is looking for. 

0. You can skip this by downloading the already patched drivers: 
    - [cnijfilter-common_2.90-1_i386.deb](https://github.com/Gamesmes90/canon-ip2600_drivers/raw/main/debian/PATCHED_cnijfilter-common_2.90-1_i386.deb)
    - [cnijfilter-ip2600series_2.90-1_i386.deb](https://github.com/Gamesmes90/canon-ip2600_drivers/raw/main/debian/PATCHED_cnijfilter-ip2600series_2.90-1_i386.deb)
1. Download the drivers from [canon's website](https://www.canon-europe.com/support/consumer_products/products/printers/inkjet/pixma_ip_series/pixma_ip2600.html?type=drivers&language=en&os=linux%20(32-bit))
2. Change libcupsys2 dependency to libcups2 containing the same library as described [in this tutorial](https://forums.linuxmint.com/viewtopic.php?t=35136) (do this to both deb files)
    - ```dpkg-deb -x name-of-package.deb tmpdir``` Extract the .deb file
    - ```dpkg-deb --control name-of-package.deb tmpdir/DEBIAN``` Extract the control file
    - Edit the dependency
    - ```dpkg -b tmpdir new-name-of-package.deb``` Rebuild the .deb file
    
## Installing
0. Install ```qemu-user-static``` if you are running this on an arm machine
1. Enable i386 repos
    ```
    sudo dpkg --add-architecture i386
    ```
    ```
    sudo nano /etc/apt/sources.list
    ```
    ```
    deb [ arch=i386 ] http://de.archive.ubuntu.com/ubuntu jammy-updates main
    deb [ arch=i386 ] http://de.archive.ubuntu.com/ubuntu jammy main universe
    ```
    ```
    sudo apt-get update
    ```
2. Install cnijfilter-common (all available dependencies should install automatically)
    ```
    sudo dpkg --install PATCHED_cnijfilter-common_2.90-1_i386.deb
    ```
    ```
    sudo apt-get install ./PATCHED_cnijfilter-common_2.90-1_i386.deb
    ```
3. Install libpng12 from [the provided .deb file](https://github.com/Gamesmes90/canon-ip2600_drivers/raw/main/debian/libpng12-0_1.2.8rel-5.1ubuntu0.3_i386.deb)
4. Install multiarch-support (libtiff4 dependency) from [the provided .deb file](https://github.com/Gamesmes90/canon-ip2600_drivers/raw/main/debian/multiarch-support_2.27-3ubuntu1.5_i386.deb)
5. Install libtiff4 from [the provided .deb file](https://github.com/Gamesmes90/canon-ip2600_drivers/raw/main/debian/libtiff4_3.9.5-2ubuntu1_i386.deb)
6. Install cnijfilter-ip2600series
    ```
    sudo dpkg --install PATCHED_cnijfilter-ip2600series_2.90-1_i386.deb
    ```
    ```
    sudo apt-get install ./PATCHED_cnijfilter-ip2600series_2.90-1_i386.deb
    ```
    
This should do it!

### Note:
Note: you may need to change the ownership of ```/usr/lib/cups/filter/pstocanonij``` as stated in [this thread](https://forum.manjaro.org/t/insecure-permissions-error-printing-to-ricoh/27268)
```
sudo chown root:root /usr/lib/cups/filter/pstocanonij
```
The related status of the printer in CUPS is:
```
Idle - “File “/usr/lib/cups/filter/pstocanonij” has insecure permissions (0100755/uid=1000/gid=1000).”
```
