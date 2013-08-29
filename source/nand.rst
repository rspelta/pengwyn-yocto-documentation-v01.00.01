.. index:: NAND memory

How to write the NAND memory
============================

This section will explain how to transfer the data from the :index:`sd card` to the Pengwyn board :index:`flash memory`.

Follow these steps:

* :ref:`Connect the Pengwyn board to the PC <usbSerial>`
* Remove :index:`jumper` :index:`J1` 

.. image:: _static/pengwynJ1.png

* Insert the already prepared SD card in the Pengwyn board socket
* Reset the board by means of button :index:`S1`

.. image:: _static/pengwynS1.png

* Press any key when U-Boot says **Hit any key to stop autoboot**

.. image:: _static/ubootStop.png

* Erase and upload the FLASH memory with the commands

::

 nand erase.chip
 run nandupdate

* Wait until the NAND write completes, than restart the board with the command

::

 reset

* Wait until U-Boot and Linux initialize the system, than insert **root** as login when required (there is no password)
* Create the flash file system with the automated script:

::
 
 ./create-nand-fs.sh

* When UBIFS is unmounted, shutdown linux with the command:

::

 shutdown -h now

* When the system has been halted, remove the SD card, insert the jumper in J1 and reset the board with button S1.

The system will now restart from NAND flash with the new operating system.
