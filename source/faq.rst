FAQ
===

What is the password of user **pengwyn**?
-----------------------------------------

::

  pengwyn

What is **sudo**?
-----------------

**sudo** is a program for Unix-like computer operating systems that allows users to run programs/commands with the security privileges of another user, normally the superuser or root. Not all the users can call sudo, only the **sudoers**, **pengwyn** user is a sudoer. When you run a command preceeded by sudo Linux will ask you the user password, for **pengwyn** user the password is **pengwyn**.

What is the password for user root?
-----------------------------------

By default, Ubuntu 10.04 comes with no password defined for **roor** user, to set it run the following command:

::

  sudo passwd root

Linux will ask you (twice, the second time is just for confirmation) to write the password for user root.

When should I use an external power supply?
-------------------------------------------

Pengwyn Board can be powered directly from the USB connector, the power consumption is about 250mA without any option installed. 
When powered from USB, the board can supply enough current to a USB keyboard (or a USB mouse) and to the DVI interface.
If you want to connect something more power hungry to the USB port, we suggest you to power the board with an external power supply or to use externally powered USB devices.

The first time I ran the virtual machine I obtained an error related to the Ethernet controller. What can I do?
---------------------------------------------------------------------------------------------------------------

We used a Realteck Ethernet controller when we built the virtual machine, if you have a different one is not a problem, click on **change network settings** and insert the proper configuration for your machine.

My build is taking too long, what can I do to optimize?
-------------------------------------------------------

Please, refer to:

* :ref:`hobSpeedup` Section of Chapter :ref:`howToUseHOB`, and/or
* :ref:`pokySpeedup` Section of Chapter :ref:`howToUsePoky`.

When I try to create the SD card with one of the provided scripts something goes wrong, what can I do?
------------------------------------------------------------------------------------------------------

The simplest reason could be that the SD card is write protected or locked, please double check that.

How do I enable commercially licensed recipes?
----------------------------------------------

Commercially licensed recipes are disabled by default.
Your conf/local.conf file, contained in your build directory (by default is /home/pengwyn/yocto/build/), has a (commented) line starting with:

::

# LICENSE_FLAGS_WHITELIST

uncomment it (delete **#** symbol).

For the details, please refer to `the official Yocto Project documentation <http://www.yoctoproject.org/docs/1.2/poky-ref-manual/poky-ref-manual.html#enabling-commercially-licensed-recipes>`_.

How do i know which drivers are included?
-----------------------------------------
* If you have built an image then you can go in *~/yocto/build/tmp/work/pengwyn-poky-linux-gnueabi/linux-pengwyn-3.2-r0/linux-pengwyn_3.2/drivers*. In this directory there are all kernel linux drivers.

* If you haven't build an image then use following commands:

::

 pengwyn@pengwyn-desktop:~$ mkdir -p /home/pengwyn/Documents/linux-kernel
 pengwyn@pengwyn-desktop:~$ cd /home/pengwyn/Documents/linux-kernel
 pengwyn@pengwyn-desktop:~/Documents/linux-kernel$ cp /home/pengwyn/yocto/poky/meta-silica/recipes-kernel/linux/linux-pengwyn-3.2/linux-pengwyn* .
 pengwyn@pengwyn-desktop:~/Documents/linux-kernel$ tar -xzf linux-pengwyn_3.2.tar.gz
 pengwyn@pengwyn-desktop:~/Documents/linux-kernel$ patch -p1 -d linux-pengwyn_3.2 < linux-pengwyn_3.2.patch
 patching file ...
 pengwyn@pengwyn-desktop:~/Documents/linux-kernel$ cd linux-pengwyn_3.2/drivers

In this directory there are all kernel linux drivers.

* Another metod is using the *menuconfig* of linux. In *linux-pengwyn_3.2* directory use the command:

::

 make menuconfig

Once opened go in *Device Drivers*.

I have problems to create a patch for the kernel. How can I do that?
--------------------------------------------------------------------
If you have already built the kernel before modify it, you need purge all file objects. To do this use the following command:

::

 make ARCH=arm mrproper

will cleanup totally the sources. See :ref:`kernel_page` for details.

The kernel doesn't load NFS root
--------------------------------
If you have modified the options in the kernel then assure that it is enabled "NFS client support" option in *File System → Network File Systems*

How do I include QWebView widget in my project? The compilation fails
---------------------------------------------------------------------
Open your .pro project file and add a new line under *QT += core gui*:

::

 QT += webkit

I have problems with internet connection
----------------------------------------
If yuor company uses proxy then read this page to configure correctly yocto:
`Working Behind a Network Proxy <https://wiki.yoctoproject.org/wiki/Working_Behind_a_Network_Proxy>`_.

What do I if change TI SDK with the VM yocto based SDK?
-------------------------------------------------------
1. remove the SD card from the Pengwyn board (if any)
2. insert jumper **J1**
3. press reset button
4. interrupt u-boot
5. run command:

::

  nand erase 0x380000 0x1000

6. insert the SD card prepared by the script
7. remove jumper **J1**
8. press reset button

I have builded a package but I do not see it in opkg list. Why?
---------------------------------------------------------------
Please, refer to :ref:`update_package_index`

