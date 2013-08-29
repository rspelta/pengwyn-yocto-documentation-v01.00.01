.. _nfs:

How to configure the remote boot
================================

Remote boot can work like in the following figure:

.. image:: /_static/remote.png

To boot from remote the host machine must share the kernel image through a tftp server and the root filesystem through an NFS server. 
Since the target will look for a dhcp during the boot, the host machine needs to have a dhcp server up and running as well.

This guide will present an example of configuration for booting the pengwyn board with a remote kernel and a remote filesystem.
To make things work you need to be sure the target can reach the host machine and all the required services (dhcp, tftp, NFS) are properly configured and up and running.

.. _networkSetup:

How to set the network
^^^^^^^^^^^^^^^^^^^^^^

The operating system that runs the needed network services (like dhcp, tftp and NFS) will be named host machine, the board that downloads from the host machine the kernel image and the root file system will be named target machine. When the target machine power on, the first program that runs is the bootloader **u-boot**. First, u-boot reads **uEnv.txt** which contains information about the server IP address, the name of the kernel image and where is located the root file system, after that downloads through tftp the kernel image and runs it with the arguments loaded before. When Linux boots it sends DHCP requests to acquire his IP address before to load the network filesystem (NFS) and runs the boot manager (systemd or init) found in root file system.

Host Machine
------------

The target must know the host ip address, so it is suggestible to assign a fixed IP address to the host machine (in this example we will use 192.168.0.20).

.. index:: DHCP

DHCP
----
You need configure your DHCP server to return a fixed IP address for the target board. 
Set the correct IP addresses in /etc/dhcp3/dhcpd.conf::

 subnet X.Y.Z.0 netmask 255.255.255.0 {
  interface eth0;
  range X.Y.Z.T X.Y.Z.T;
  option subnet-mask 255.255.255.0;
  option broadcast-address X.Y.Z.255;
  option routers X.Y.0.1;
 }

| by replacing **X.Y.Z** with the correct IP address space (e.g. 192.168.0), and **T** with the address used by target board. (e.g. 101).
| If you are not using the interface eth0, replace eth0 with the correct interface name.

Finally, the dhcp demon must be restarted::

 /etc/init.d/dhcp3-server restart

.. index:: uEnv.txt

SD-CARD & uEnv.txt
------------------
You need to have a bootable SD-CARD with u-boot and MLO installed in the FAT partition. Refer to :ref:`sdcard` page on how to create the SD card.

Copy uEnv.txt file located in */home/pengwyn/yocto/remote/uEnv.txt* to the boot partition. The file has the following structure::

 ipaddr=X.Y.Z.T
 serverip=X.Y.Z.H
 gatewayip=X.Y.Z.G
 netmask=255.255.255.0
 hostname=pengwyn
 rootpath=/home/pengwyn/yocto/remote/nfs-eclipse-rootfs
 bootfile=uImage
 net_args=run bootargs_defaults;setenv bootargs ${bootargs} root=/dev/nfs nfsroot=${serverip}:${rootpath},${nfsopts} rw ip=${ipaddr}:${serverip}::${netmask}:${hostname}:${nfsdevice};
 tftp_nfs_boot=echo Booting from network...; tftp ${loadaddr} ${serverip}:${bootfile}; run net_args; bootm ${loadaddr};
 uenvcmd=run tftp_nfs_boot;

| Replace **X.Y.Z** with correct IP address space (e.g. 192.168.0).
| Replace **X.Y.Z.T** with the IP address used by the Target board (e.g. 101).
| Replace **X.Y.Z.H** with the IP address used by the Host (e.g. 20).
| The gatewayip is not actually used.

Files to share
^^^^^^^^^^^^^^

Use ref poky or ref hob to build your preferred root file system and the kernel image. You can find more detailed information on how to do that in the official documentation provided by the the Yocto Project

.. index:: NFS

Root FS
-------
To make the NFS service work, you have to provide a root filesystem. After you built your preferred filesystem with the Yocto toolchain, you need to extract the corresponding .tar.gz into a predefined directory:

*~/yocto/remote/nfs-eclipse-rootfs*

Assuming you are working inside directory *~/yocto/build/* you can setup the root filesystem copying the proper .tar.gz from directory
*~/yocto/build/tmp/deploy/images/* 
to directory
*~/yocto/remote/nfs-eclipse-rootfs*
and than decompressing and untarring (with **superuser** privileges) the .tar.gz file.

For example, if you were interested to the filesystem image LSB SDK, after the build process you get the file  *core-image-lsb-sdk-pengwyn.tar.gz*::, so you can copy that file in the directory where the NFS service expects it:

::

 cp ~/yocto/build/tmp/deploy/images/core-image-lsb-sdk-pengwyn.tar.gz ~/yocto/remote/nfs-eclipse-rootfs

and finally you can decompress and untar it with **superuser** privileges like with the following commands:

::

 cd ~/yocto/remote/nfs-eclipse-rootfs
 sudo tar -zxf core-image-lsb-sdk-pengwyn.tar.gz

Remember, the password of user pengwyn is: **pengwyn**

.. index:: TFTP

Kernel Image
------------
To run the kernel image onto the target board, uboot will download it from the tftpboot directory on the host machine. In the same directory were you have the rootfs images (if you are building the images with bitbake/hob from ~/yocto/build directory, then the directory containing the images will be ~/yocto/build/tmp/deploy/images) there is the kernel image as well named uImage.bin. Copy it in ~/yocto/remote/tftpboot and rename it in uImage.

::

 sudo cp ~/yocto/build/tmp/deploy/images/uImage.bin ~/yocto/remote/tftpboot/uImage

Boot up
^^^^^^^
You must connect target and host machines together by means of a network interface, one possible network schema is to connect both of them with the same ethernet cable through a point to point connection, the other possible schema is to connect them to the same LAN. With the last connection schema, double check that the IP addresses specified within file1, file2, file3 are compliant with your network address space.
To get all the console messages at boot and, at the end of the boot process, a command prompt, connect the Pengwyn board via USB to your computer.

.. index:: Minicom

.. _usbSerial:

Usb-Serial
----------

On your Host Operating System (not on guest operating system running with the Virtual Machine) you need to have a serial communication program like minicom (for Linux as host operating systems) or HyperTerminal (for Windows as host operating system). In this document only the setup of minicom program will be treated. 

The required steps to get the usb-serial link work are:

1. clean the kernel messages buffer with the following command:

::

 sudo dmesg -c

2. connect the Pengwyn board to the PC with mini-USB cable near DVI connector.

3. determine the serial device name with this command:

::

 dmesg | grep ttyUSB

on the standard output you will see something like:

::

 [11401.006607] usb 1-1.1: FTDI USB Serial Device converter now attached to ttyUSB0

In such an example, ttyUSB0 is the serial device name, and /dev/ttyUSB0 is the serial device

3. run command (sudo password is **pengwyn**)
::

 sudo minicom -w -s

4. select *select port setup* and press enter.

5. setup the port with the following configuration:

::

 A -    Serial Device      : /dev/ttyUSB0
 B - Lockfile Location     : /var/lock
 C -   Callin Program      :
 D -  Callout Program      :
 E -    Bps/Par/Bits       : 115200 8N1
 F - Hardware Flow Control : No
 G - Software Flow Control : No

6. once you are done configuring the serial port, you are back to minicom main menu and you can select *exit*.

7. Control that the SD card is in the slot of Pengwyn board if is in it, you can press the reset button.

8. if everything has been properly configured, the target board will download the kernel image

::

 Booting from network...                                                         
 link up on port 0, speed 100, full duplex                                       
 link up on port 1, speed 100, full duplex                                       
 Using cpsw device                                                               
 TFTP from server 192.168.0.20; our IP address is 192.168.0.101             
 Filename 'uImage'.                                                              
 Load address: 0x80200000                                                        
 Loading: ################################################################# 

9. and the Linux kernel will mount the root filesystem by means of NFS:

::

 [    5.791564] Sending DHCP and RARP requests . OK                              
 [    5.802032] IP-Config: Got DHCP answer from 192.168.0.20, my address is 192.168.0.101                                                                   
 [    5.813079] IP-Config: Complete:                                             
 [    5.816436]      device=eth0, addr=192.168.0.101, mask=255.255.255.0, gw=255.255.255.255,                                                                  
 [    5.824951]      host=pengwyn, domain=, nis-domain=(none),                   
 [    5.830657]      bootserver=126.126.126.136, rootserver=192.168.0.20, rootpath=                                                                           
 [    5.848632] VFS: Mounted root (nfs filesystem) on device 0:16.


10.  finally, the login will appear:

::

 Yocto (Built by Poky 7.0.1) 1.2.1                                               
  ttyO0                                                                          
                                                                                
 pengwyn login: 

Login with username root, no password is required.


