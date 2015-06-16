Powersave
=========

This is a powersaving script for Linux. Forked from https://github.com/Unia/powersave. This script is laptop specific (I currently own an Acer ). That means that this script will **not** work on every setup so please **check** this before running it on your computer! Things might break otherwise and I will **not** be responsible for any damage that has been done.

Installation
------------

The script will need the following dependencies to run:
* hdparm
* wireless_tools
* xset
* udev

Just run `make install` as root to install the script and the udev rules.

Usage
-----

Once the script is installed, one can run `powersave true | false` and the powersaving will kick in. The udev rule will automatically run these commands depending on the battery state (Charging, discharging). The other udev rule will also enable or disable CD polling by udisks, depending on the battery state.

Configuration
-------------

Things you should edit, should you use this on your setup:
* NMI watchdog; If you want to disable this at kernel level too, use: `nmi_watchdog=0`
* ASPM powersave; To be able to write to this file you have to use a kernel parameter at boot: `pcie_aspm=force`
* Disk powersave; I have removed hdparm because I have an SSD (laptop). If you have multiple HDD's and want to run powersave on all of them, then use this:
  `for dev in $(awk '/^\/dev\/sd/ {print $1}' /etc/mtab); do hdparm -S 6 -B 254 -a 2048 $dev; done`  
Also, note that using -B254 is not power-efficient. If you want maximum powersave, use -B1. **THIS CAN HURT YOUR HARDDRIVE, SO BE CAREFUL.** See https://wiki.archlinux.org/index.php/Laptop#Hard_drive_spin_down_problem
* Battery line in the systemd sleep hook, your setup might have a different path.

These modules can be blacklisted to save power:
* btusb
* bluetooth
* uhci_hcd
* pcmcia
* yenta_socket
* ipw2200
* ohci1394
* firewire-ohci

In Arch Linux, add this list to `/etc/modprobe.d/blacklist.conf` to have them all blacklisted at boot.

ToDo
----

* Set more aggresive power savings
