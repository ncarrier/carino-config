# Configuration files for the carino project

## Overview

This project contains the build time and run time configuration files for the
carino project.

## Details on files

### build\_config

Defines the values of the variables needed by the carino build system.

### busybox\_config

Build time configuration for the busybox package. Please refer to
build\_scripts/busybox.carbuild for instructions on how to update this file.

### firmware/rtlwifi/rtl8192cufw\_TMSC.bin

Firmware for the wifi dongle used by the car.

### hostapd\_config

Build time configuration for the hostapd package.

### kernel\_config

Build time configuration for the linux kernel package. Launch the linux-config
build target to update.

### packages

List of the packages to be built when issuing the `bb` command. Please remember
that the order is preserved, so dependencies must be placed before the packages
they are a dependency of.

### README.md

Well... This file...

### uEnv.txt

Run time configuration for u-boot. Modify this file if you want, for example,
to modify the kernel command-line.

### skel

Directory whose content is merged to the rootfs after each build. Place here the
files which are not considered as being part of a package, but which must be
present on target, e.g. run time configuration files, small helper scripts.  
The skel is also used to create the base tree structure for the root fs. If you
want a directory to be present at runtime, as soon as / is mounted, create it
here. As empty directories can't be tracked by git, one needs to create an empty
.gitignore in the directory and add it to git.

### skel/bin/config_network

Hotplug helper, called by the kernel on peripheral events. Used for statically
configuring the wifi adapter.

### skel/bin/service, skel/bin/stop, skel/bin/fake_service and skel/bin/restart

These scripts are utilities for managing the services which busybox's init's
inittab monitors. These are respawned upon exit. stop replaces the service's
executable with a link to fake_service, which performs a simple sleep, then
kills the original service. init takes care of "re-launching" the service, in
fact, fake_service. Restart does the opposite, reinstalls the original binary
and kills the "service", which is restarted by init. service is the
implementation of restart and stop. Please keep in mind that this feature is a
debug feature only. It only works when the root file system is mounted RW and
suffers serious bugs, notably, when a service is stopped, if the target reboots,
then it won't start again until an explicit call to restart, which is
problematic for example for hostapd, udhcpd or adbd...

### skel/bin/adbd.conf

Configuration file for the adbd daemon, replacing the android properties system,
which the libc and init process, used in carino are lacking. The most important
entry is ro.secure, which, once set to 1, forces adbd to drop it's privileges.

### skel/etc/fstab

Static information about the filesystems.

### skel/etc/hostapd.conf

Run time configuration file for hostapd.

### skel/etc/init.d/rcS

Script ran at the end of the boot sequence by init. Performs the end of the
configuration, which fstab can't take care of, configures lo and tries to
configure wlan0 if already there.

### skel/etc/inittab

Read by init at startup. Instructs it to mount the filesystems, to run the rcS,
then to start and monitor the system services.

### skel/etc/profile

Loaded at a shell's startup. Convenient for configuring environment variables
or aliases.

### skel/etc/udhcpd.conf

Configuration for udhcpd, the DHCP server.

## License

    This file is part of the Carino project documentation.
    Copyright (C) 2015
      Nicolas CARRIER
    See the file doc/README.md for copying conditions

