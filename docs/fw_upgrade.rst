======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is:

- 

If you are on an older version, please contact ANELLO for the latest FW image.

FW upgrades currently must be done using both the serial RS-232-1 port and ethernet port and can be done on Windows and Linux environments

Please ensure power, serial, and ethernet connection is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, please power cycle the unit and try again.

Firmware Upgrade Procedure - Windows over RS232-1 only
-------------------------------------------------------
1. Connect ethernet to computer and open QGround Control (to connect to QGroundControl with ethernet, computer IP adress must be set to 192.168.0.2, subnet mask 255.255.255.0)
    Detailed instructions for connecting to QGroundControl can be found in the `Maritime INS Getting Started Guide <https://docs-a1.readthedocs.io/en/maritime_ins/getting_started_maritimeins.html>`_

2. In Mavlink console (Q > Analyze Tools > Mavlink Console):

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
        python px_uploader.py --port COM5 --baud-bootloader 115200 /Users/user1/Downloads/anello_maritime_default.px4
        # Change "COM5" to match your port that the Maritime INS is plugged into
        # Change "/Users/user1/Downloads/anello_maritime_default.px4" to the path to the ANELLO-provided FW image (.px4 file) on your local computer

8. After it completes, you will see "Rebooting. Elapsed Time x.x" - this means the FW upgrade was successful


*For support upgrading firmware with Linux, contact info@anellophotonics.com*