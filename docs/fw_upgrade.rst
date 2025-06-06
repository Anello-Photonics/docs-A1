======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is:

- 

If you are on an older version, please contact ANELLO for the latest FW image.

FW upgrades currently must be done over the serial interface (RS232-1) and can be done on Linux environments

Please ensure power and serial connection is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, please power cycle the unit and try again.

Firmware Upgrade Procedure - Windows over RS232-1 only
-------------------------------------------------------
1. Connect ethernet to computer and open QGround Control (to connect to QGroundControl with ethernet, computer IP adress must be set to 192.168.0.2, subnet mask 255.255.255.0)

2. In Mavlink console:

.. code-block:: python
    :caption: Mavlink console

    reboot -b

3. Close QGroundControl
4. Plug into RS232-1
5. Download ANELLO-provided FW image (.px4 file) onto your local computer
6. Open device manager, and find which COM port is the serial connection.
7. In terminal:

.. code-block:: python
    :caption: Terminal
    
        # cd into folder with px4_uploader.py
        ./Tools/px_uploader.py --port /dev/ttyUSB0 --baud-bootloader 115200 /Users/user1/Downloads/anello_maritime_default.px4
        # Change "/dev/ttyUSB0" to match your port that the Maritime INS is plugged into
        # Change "/Users/user1/Downloads/anello_maritime_default.px4" to the path to the ANELLO-provided FW image (.px4 file) on your local computer

8. After it completes, you will see "Rebooting. Elapsed Time x.x" - this means the FW upgrade was successful


Firmware Upgrade Procedure - Linux over RS232-1 only
-----------------------------------------------------

1. In Mavlink console:

.. code-block:: python
    :caption: Mavlink console

    reboot -b


2. Plug into RS232-1
3. Download ANELLO-provided FW image (.px4 file) onto your local computer
4. In terminal:

.. code-block:: python
    :caption: Terminal

        # run the following to find the USB port # that the Maritime INS is plugged into: 
        sudo ls /dev/ttyUSB*
        # cd into PX4-Autopilot repo
        ./Tools/px_uploader.py --port /dev/ttyUSB0 --baud-bootloader 115200 --baud-flightstack 115200 /Users/user1/Downloads/anello_maritime_default.px4
        # Change "/dev/ttyUSB0" to match your port that the Maritime INS is plugged into
        # Change "/Users/user1/Downloads/anello_maritime_default.px4" to the path to the ANELLO-provided FW image (.px4 file) on your local computer

5. After it completes, you will see "Rebooting. Elapsed Time x.x" - this means the FW upgrade was successful
 
FW upgrade over Windows Subsystem for Linux: 
--------------------------------------------------------------------------------------------

Connecting to USB in WSL 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow the instructions at `Microsoft <https://learn.microsoft.com/en-us/windows/wsl/connect-usb>`_ 

Initial Setup 
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install usbipd .msi file at `Github <https://github.com/dorssel/usbipd-win/releases>`_ 

Follow all instructions


Required Steps each power up 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Windows command line refers to the terminal windows OS. The linux command line refers to the command line within WSL. Steps 1-3 are all on the windows command line. 

This needs to be done every time the system restarts or the device is plugged in 

1. Get bus id 

    a. With the device not plugged in run “usbipd list” 

    .. code-block:: python
        :caption: Terminal

            usbipd list

    b. Plug in the device 

    c. Run “usbipd list” again 

    .. code-block:: python
        :caption: Terminal

            usbipd list

    d. The new row in the second use is the evk. 

    e. The First column should be labelled BUSID. That code is the bus id (ex: "1-1")

2. Bind the device with “usbipd bind --busid <busid>” 

    a. Replace <busid> with the value from step 1. 

    b. Example: “usbipd bind --busid 1-1” 

    .. code-block:: python
        :caption: Terminal

            usbipd bind --busid 1-1

    c. Run “usbipd list” to make sure it worked. The evk row should now say shared under the “STATE” column

    .. code-block:: python
        :caption: Terminal

            usbipd list 

3. Attach the device with “usbipd attach --wsl --busid <busid>” 

    a. Replace <busid> with the value from step 1. 

    b. Example: “usbipd attach --wsl --busid 1-1”

    .. code-block:: python
        :caption: Terminal

            usbipd attach --wsl --busid 1-1

4. Check its working within linux by typing “ls /dev/ttyUSB*” in the linux command line. Multiple ports should showup. 

    .. code-block:: python
        :caption: WSL Terminal

            ls /dev/ttyUSB*


After USB ports are attached to WSL, steps for FW upgrade on Linux can be followed from WSL terminal.
