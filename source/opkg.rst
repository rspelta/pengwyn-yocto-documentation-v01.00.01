Opkg Basics
===========

.. image:: _static/opkg.png
   :align: left

| 
| Opkg (Open PacKaGe Management) is a lightweight package management system. It is written in C and resembles apt/dpkg in operation. It is intended for use on embedded Linux devices and is used in this capacity in the OpenEmbedded and OpenWrt projects. 
| 
|

Useful commands:

- command to know what packages are installed on the file system:

::

  opkg list-installed 

- show where are the files installed of the packet:

::

  opkg search name_packet 

- show what packets depend on the “name_packet” package:

::

  opkg whatdepends name_packet 

- remove packages:

::

  opkg remove name_packet

for this command there are important options:

1. This option will force the removal of the package but will leave any packages that depend on this package installed:

::

  -force-depends

2. This option will go up the dependency list and remove all packages in the dependency chain:

::

  -force-removal-of-dependent-packages 

- install the packages:

::

  opkg install name_packet 

How to add a repository
-----------------------

Install a web server in the virtual machine, e.g.:

::

  sudo apt-get install apache2

Copy the built packages inside the web server directory, e.g.:

::

  sudo cp -r /home/pengwyn/yocto/build/tmp/deploy/ipk /var/www

Create a file named *<something>.conf* (e.g. mine-repositories.conf) under */etc/opkg/* of the Pengwyn file system and fill it with the following lines:

::

  src/gz arm     http://192.168.0.20/ipk/armv7a-vfp-neon
  src/gz all     http://192.168.0.20/ipk/all
  src/gz pengwyn http://192.168.0.20/ipk/pengwyn

.. note::

  192.168.0.20 is the server IP where there is the web server with repository

after that run the following command from Pengwyn's shell:

::

  opkg update


Now you are ready to download and install the packages from the network.

Tip
---

With "opkg list | wc -l" you can know approximately how many packets there are in repository

::

  opkg list | wc -l
  opkg update
  opkg list | wc -l

If the updating got success with the last command you see the number of packets incremented.

.. _update_package_index: 

How to update the repository in the vm
--------------------------------------

| Once you have builded a package by Hob, do you need update package index. To do this, double click to the icon **Update packages index**.

.. image:: _static/upi.png

| after that run the following command from Pengwyn's shell:

::

  opkg update
