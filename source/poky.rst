.. _howToUsePoky:

How to use Poky
===============

**Poky** is a stable version of **OpenEmbedded** tailored for the **Yocto Project**. It consists of **BitBake**, that is a make-alike build tool, and a set of recipes (.bb files) and configuration files used to drive bitbake during the build process.
A recipe is a collection of metadata used by BitBake to set variables or define additional build-time tasks. With the variables, a recipe can specify, for example, where to get the sources, which build process to use, the license of the package, an so on. There is a set of predefined tasks (the fetch task for example fetches the sources from the network, from a repository or from the local machine, than the sources are cached for later reuses), but a recipe can always adds custom ones or overrides/modifies existing ones. **Yocto** is an umbrella project that contains a few components, Poky is one of the largest components of Yocto. This document will focus on the most basic topics, for a full guide you are encouraged to read `the official documentation of the Yocto Project <https://www.yoctoproject.org/documentation>`_. 

.. _prepareEnvironment:

How to prepare the environment
------------------------------
The virtual machine contains Denzil (version 7.0.1 of Poky, corresponding to Yocto version 1.2.1) already installed and configured to work with the Pengwyn board under */home/pengwyn/yocto/poky*. To start using Poky, open a terminal (Ctrl+Alt+t or from Ubuntu's menu: Applications→Accessories→Terminal) and type:

::

 pengwyn@pengwyn-desktop:~$ cd yocto
 pengwyn@pengwyn-desktop:~/yocto$ source poky/oe-init-build-env

 ### Shell environment set up for builds. ###
 
 You can now run 'bitbake <target>'

 Common targets are:
 core-image-minimal
 core-image-sato
 meta-toolchain
 meta-toolchain-sdk
 adt-installer
 meta-ide-support

 You can also run generated qemu images with a command like 'runqemu qemux86'

 pengwyn@pengwyn-desktop:~/yocto/build$

With the *source* command the environment has been prepared and, in the current shell, you are able to execute bitbake commands. The sourced script changed also the current directory, that now is *~/yocto/build*.
You are free to use another build directory, to do that, when you source the environment script, you can specify the name of the directory to use like in the following example:

::

 pengwyn@pengwyn-desktop:~/yocto$ source poky/oe-init-build-env myBuilds
 You had no conf/local.conf file. This configuration file has therefore been
 created for you with some default values. You may wish to edit it to use a 
 different MACHINE (target hardware) or enable parallel build options to take 
 advantage of multiple cores for example. See the file for more information as 
 common configuration options are commented.

 The Yocto Project has extensive documentation about OE including a reference manual
 which can be found at:
    http://yoctoproject.org/documentation

 For more information about OpenEmbedded see their website:
    http://www.openembedded.org/

 You had no conf/bblayers.conf file. The configuration file has been created for
 you with some default values. To add additional metadata layers into your
 configuration please add entries to this file.

 The Yocto Project has extensive documentation about OE including a reference manual
 which can be found at:
    http://yoctoproject.org/documentation

 For more information about OpenEmbedded see their website:
    http://www.openembedded.org/



 ### Shell environment set up for builds. ###

 You can now run 'bitbake <target>'

 Common targets are:
    core-image-minimal
    core-image-sato
    meta-toolchain
    meta-toolchain-sdk
    adt-installer
    meta-ide-support

 You can also run generated qemu images with a command like 'runqemu qemux86'

 pengwyn@pengwyn-desktop:~/yocto/myBuilds$

How to build your Linux distribution
------------------------------------

To make a Linux distribution for the Pengwyn board you are going to need:


* a bootloader,
* the Linux Kernel (along with modules), and
* a filesystem.

There is a set of recipes already prepared to generate all that. The pre-configured virtual machine supports the following subset of those recipes:

* core-image-base.bb
* core-image-basic.bb
* core-image-core.bb
* core-image-clutter.bb
* core-image-minimal.bb
* core-image-minimal-dev.bb
* core-image-minimal-mtdutils.bb
* core-image-lsb.bb
* core-image-lsb-dev.bb
* core-image-lsb-sdk.bb
* qt4e-demo-image.bb
* core-image-sato.bb

     

Each recipe generates the bootloader, the Linux Kernel, and a specific filesystem. We encourage you to read the official documentation for the details of the aforementioned images recipes. To tell BitBake to build one of those images, run *bitbake* with the image recipe name (without .bb) as argument, for example, to build the core-image-minimal.bb image run *bitbake core-image-minimal* like in the following example:

::

 pengwyn@pengwyn-desktop:~/yocto/build$ bitbake core-image-minimal
 Pseudo is not present but is required, building this first before the main build
 Loading cache: 100% |#############################################| |ETA:  00:00:00
 Loaded 1790 entries from dependency cache.

 OE Build Configuration:
 BB_VERSION        = "1.15.2"
 TARGET_ARCH       = "arm"
 TARGET_OS         = "linux-gnueabi"
 MACHINE           = "pengwyn"
 DISTRO            = "poky"
 DISTRO_VERSION    = "1.2.1"
 TUNE_FEATURES     = "armv7a vfp neon cortexa8"
 TARGET_FPU        = "vfp-neon"
 meta              
 meta-yocto        = ":"
 meta-efl          
 meta-gnome        
 meta-gpe          
 meta-initramfs    
 meta-multimedia   
 meta-oe           
 meta-xfce         = "(nobranch):e77646bc2cf905fd671fc5c19ab6fd17f9b94b3f"
 meta-ti-amsdk     = "(nobranch):d8f257b275bbf2d573d66d823a118c765f7a26f9"
 meta-silica       = ":"

 NOTE: Resolving any missing task queue dependencies
 NOTE: Preparing runqueue
 NOTE: Executing SetScene Tasks
 NOTE: Executing RunQueue Tasks
 ...


Depending on your hardware and Internet characteristics, this task can take from less than one hour to several hours. Once it finishes, you are going to find bootloader, Linux kernel, kernel modules and filesystem under directory *~/yocto/tmp/deploy/images*. If you were working under the default directory (build directory), for example, you are going to find all the following files under directory *~/yocto/build/tmp/deploy/images*:

* **MLO** (:index:`bootloader`),
* **u-boot-pengwyn.img** (:index:`bootloader`),
* **uImage-pengwyn.bin** (:index:`Linux Kernel`), 
* **modules-3.2.0-r0-pengwyn.tgz** (:index:`Kernel modules`), and
* **core-image-minimal-pengwyn.tar.gz** (filesystem to be written onto the SD card, the NFS or the NAND memory),

that is the result of building your Linux system from the ground up.
It might happen that a fetch task gives an error, if so, double check that the virtual machine has a proper network configuration. If the network configuration has been proved correct, the error might mean that the needed server is down for some reason, in that case the only option you have is to wait and try again later.

.. _pokySpeedup:

How to speedup the build process
--------------------------------

When you imported the virtual machine, you might have choosen the number of processors to give to the virtual machine, if so, you can change the parallelism factors to speedup the build process.
Once you sourced the environment script and it changed your current directory, you can find the file local.conf inside directory conf, use your preferred editor to open it and change BB_NUMER_THREADS variable value from *2* to *<number of processors> * 2*, and change PARALLEL_MAKE variable value from *-j 2* to *-j <number of processors> * 2*. For example, if you gave 4 processors to the virtual machine, you could set:

* BB_NUMBER_THREADS = 8, and
* PARALLEL_MAKE = -j 8.

Those modifications are available to the current build directory alone, so if you create another build directory you should set again the same values in the corresponding local.conf file.
To make such changes available anytime you create a new build directory, you can edit file ~/yocto/poky/meta-yocto/conf/local.conf.sample and change the value of the aforementioned variables, every time you create a new build directory local.conf.sample file will be used to create the project local.conf file, and the modification will be seen and used by your new projects.

Package management and class
----------------------------

Poky supports the generation of three types of packages:

* ipk,
* rpm, and
* deb.

The default one is ipk, you can change that modifying the PACKAGE_CLASSES variable value inside conf/local.conf file in your build directory. Also in this case, you can make your preference permanent editing file ~/yocto/poky/meta-yocto/conf/local.conf.sample directly.

The tool used to manage the packages directly onto the target is going to be installed by default, to remove it and save space on the generated file system you can edit file local.conf and remove *package-management* value from EXTRA_IMAGE_FEATURES variable. As usual, if you want that modification to be permanent, change the value directly within file local.conf.sample.


Development accessories
-----------------------

To debug your code on the Pengwyn board you will need:

* the *tcf-agent* service, and
* *gdbserver*.

Both the utility have already been configured to be installed on the destination file system but, if you want a smaller file system and you already finished debugging your application, you can remove *tcf-agent* and *gdbserver* values from IMAGE_INSTALL_append inside local.conf file.

Another set of utilities that you might be insterested about is *mtd-utils*, with the programs installed by such a package you can manage the NAND memory and you can write your filesystem directly onto the NAND memory. By default the *mtd-utils* package is going to be installed onto your preferred filesystem, anyway, if you want to remove it from the build process you can delete *mtd-utils* value from IMAGE_INSTALL_append variable.

Kernel modules
--------------
Kernel modules are pieces of code that can be loaded and unloaded into the kernel space upon demand. They extend the functionality of the kernel without the need to reboot the system. If you want insert new modules into your distribution there are two ways to do this:

* manually (after you have built the image):

 1. turn on Pengwyn board

 2. use minicom (refer to section :ref:`usbSerial` to know how to configure minicom) to uncompress the new module package from the root of the target file system.

 3. launch command:

 ::

  depmod

This command handle dependency descriptions for the new loadable kernel modules. This dependency is written to *modules.dep* file that can be used by *modprobe* to automatically load the relevant modules.

* automatically (before to build image):

 1. Before to use bitbake command to build a image, go to directory *~/yocto/build/conf* and open the local.conf file with your preferred editor.

 2. Modify **IMAGE_INSTALL_append** variable value adding **kernel-modules** like in the following example:

::

 IMAGE_INSTALL_append = " tcf-agent dgbserver kernel-modules"

In this way the modules dependencies will be resolved at build time and the kernel modules will be installed in the file system.
