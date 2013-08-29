Qt SDK
======

.. image:: _static/qt-0.png
	   :align: left

| **Qt** is a cross-platform application framework that is used for developing application software with a graphical user interface (GUI). 
| **Qt Creator** is a cross-platform C++ IDE, it includes a visual debugger and an integrated GUI layout and forms designer. 
| The versions used in this SDK are **Qt SDK 4.7.4** and **Qt Creator 2.4.0**.
| It is possible to compile applications for **x86** and **ARM** processors. 
| You can debug the program on the virtual machine or on Pengwyn Board.
|

.. note::

 Before reading this Chapter you should be able to use HOB, bitbake, NFS and minicom (or a similar program).

Build image with qt
-------------------

1. With HOB or bitbake build **qt4e-demo-image**. To see how to do this, refer to :ref:`howToUseHOB` and/or :ref:`howToUsePoky` Chapters.

2. Once the image has been built (and assuming your current build directory is */home/pengwyn/yocto/build/*), run the following commands:

::

 cd /home/pengwyn/yocto/remote/nfs-eclipse-rootfs/
 sudo rm -rf *
 cp /home/pengwyn/yocto/build/tmp/deploy/images/qt4e-demo-image-pengwyn.tar.gz .
 sudo tar -xzf qt4e-demo-image-pengwyn.tar.gz

3. Open file */home/pengwyn/yocto/remote/nfs-eclipse-rootfs/etc/inittab* and comment line 41:

::

 # 1:2345:respawn:/sbin/getty 38400 tty1

this allows a USB keyboard to be seen by your Qt application. 

4. We don't need the qt demo application to start at boot, run the following command:

::

 sudo rm /home/pengwyn/yocto/remote/nfs-eclipse-rootfs/etc/init.d/qt4demo

5. Turn on Pengwyn board.

Hello World!
------------

The purpose of this example project is to generate a form with an "Hello World" label in it, at the beginning on the x86 virtual machine and than on the Pengwyn board.

To create the project follow these steps:

1. Launch Qt Creator either from the **Welcome Screen** (**Develop->IDEs->Qt Creator**)

.. image:: _static/qtCreatorStart.png

or from the Desktop icon

.. image:: _static/run_qt.png

or from the command line

::

 run_qtcreator

2. Go to *File -> Open File or Project* to open **QtHelloWorld.pro** file located in *~/workspace/qt/QtHelloWorld/* directory.

3. Click on "QtHelloWorld" icon to open project menu.

.. image:: _static/qt-1.png

4. Select the build configuration: **Qt 4.7.4 (Qt-4.7.4) Debug**.

.. image:: _static/qt-2.png

5. To build the project, click on the bottom-left icon.

.. image:: _static/qt-3.png

6. Once you built the project, click on the green triangle to run it.

.. image:: _static/qt-4.png

7. Congratulations! You just built your first Qt application for x86.

.. image:: _static/qt-5.png

In the next section we will debug our Hello World! application directly on Pengwyn.

Debug Hello World project on pengwyn board
------------------------------------------

8. Select build configuration: **Qt 4.7.4 (Qt-4.7.4-arm) Debug** and build the project.

.. image:: _static/qt-10.png

9. Copy the generated executable to *~/yocto/remote/nfs-eclipse-rootfs/home/root*.

::

  sudo cp ~/workspace/qt/QtHelloWorld-build-desktop-Qt_4_7_4__Qt-4_7_4-arm__Debug/QtHelloWorld ~/yocto/remote/nfs-eclipse-rootfs/home/root

10. Use minicom to launch gdbserver application on the target board:

::

  gdbserver :10000 QtHelloWorld -qws

11. | In Qt Creator, open the source file main.cpp and set a breakpoint at line 6. 
    | To do this go with the mouse at line 6 and click with the right button to open the menu, select **Set brackpoint at line 6**

.. image:: _static/qt-6.png

12. Go to *Debug→Start Debugging→Attach To Remote Debug Server*, a form named "Start Debugger" will appear, insert the following data:

.. image:: _static/qt-7.png

- Debugger: **/opt/poky/1.2.1/sysroots/i686-pokysdk-linux/usr/bin/armv5te-poky-linux-gnueabi/arm-poky-linux-gnueabi-gdb**

- Local executable: **/home/pengwyn/workspace/qt/QtHelloWorld-build-desktop-Qt_4_7_4__Qt-4_7_4-arm__Debug/QtHelloWorld**

- Host and port: **192.168.0.101:10000**

- Architecture: **arm**

- GNU target: **auto**

- Sysroot: **/home/pengwyn/yocto/remote/nfs-eclipse-rootfs**

Press **OK** button to start the debug.

.. image:: _static/qt-8.png

13. The hotkeys to debug the application are:

- **F10**: Step over

- **F11**: Step into

- **Shift + F11**: Step out

- **F5**: Continue, or press this icon:

.. image:: _static/qt-9.png

14. To successfully exit from the debug it is better to close the graphical application from the target board with the mouse by clicking on the 'X' symbol. 

