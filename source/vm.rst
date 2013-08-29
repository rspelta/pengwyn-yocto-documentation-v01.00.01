Installing the Virtual Machine
==============================

The development environment is provided as a Virtual Machine image. 

To be able to use it, you first need to install VirtualBox. The version must be the 4.2.10 or above. 

.. image:: _static/virtualboxlogo.png
   :align: left

Go to:

https://www.virtualbox.org/wiki/Downloads

and download the version that suits your host operating system. You need to download and install the Extension Pack as well.

.. important::
   Make sure that the extension pack has the same version of VirtualBox.

Install the software with all the default options.

Launch the program and follow these steps: 

.. tip::
   If you are using Linux, double click directly on the .ova file from your file explorer, there will appear a window so click import button.
   
   .. image:: _static/importova.png
   
   Jump to step 3.

1. From the menu: *File → Import Appliance*

.. image:: /_static/importAppliance.png

2. Click on “Open appliance…” button and select the .ova file “PengwynYocto-beta-2013-03-19.ova”.

3. After opening the appliance, click on “Shared Folders” and select a folder to share with your host operating system. When edit shard folder select **Auto-mount** check box.

.. image:: /_static/vbSharedFolders.png

4. The ethernet card must be attached to the LAN, not to the WLAN (to make some test you will connect the Pengwyn board to the PC with an ethernet point-to-point connection). To set the correct card, go to menu "machine -> Settings".
   Click on "Network" tab and select your LAN card. Double check also that the field "Attached to" is set to "Bridged Adapter".
   If you want to improve the performances of the virtual machine, please read :ref:`cpuRam`.
   Click on button "Ok" to apply your choices when you are done.

.. image:: /_static/ivs2.png

.. important::
   The sudo password is **pengwyn**

5. Click the icon "Start" button on the toolbar.

.. image:: /_static/vbStart.png

How to configure the network on the virtual machine
---------------------------------------------------

Another important thing to do is to set the IP address of the virtual machine as static. To do this follow the next steps inside the virtual machine:

1. Right-click on network connection icon. Select *Edit Connections...*.

.. image:: /_static/ip-1.png

2. In *Wired" tab, select "Auto eth0" and press *Edit...* button.

.. image:: /_static/auto-eth0.png

3. Click to *IPv4 Settings*, press on *Add* button and insert the following address:

* *Address*: 192.168.0.20

* *Netmask*: 255.255.255.0

* *Gateway*: none

.. image:: /_static/edit-connection.png

4. Click on Apply.

.. _cpuRam:

How to setup the number of CPUs and the amount of RAM used by your virtual machine
----------------------------------------------------------------------------------

You can configure the system settings of the virtual machine. This is possible only if the virtual machine is off. First, select the Yocto based SDK virtual machine from the list of virtual machines, click on the  *Settings* icon in the *Oracle VM VirtualBox Manager* window:

.. image:: /_static/setvm-1.png

In the left menu, click on *System*. In the *Motherboard* tab you can select how much **RAM** you want to assign to the vm.

.. image:: /_static/setvm-2.png

Select *Processor* tab to select how many **CPUs** you want to assign to the virtual machine:

.. image:: /_static/setvm-3.png

If you changed the number of processors, you might want to consider reading the guides on how to speedup the build process for :ref:`Poky <pokySpeedup>` and :ref:`HOB <hobSpeedup>`.
