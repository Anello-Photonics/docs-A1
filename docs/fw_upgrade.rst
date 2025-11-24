======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is v1.1.9. 
If you are on an older version, please contact ANELLO for the latest FW image.

The ANELLO FW upgrade python script can be found in the following public GIT repo: `ANELLO_INS_Scripts <https://github.com/Anello-Photonics/ANELLO_INS_Scripts.git>`__

Firmware Upgrade Procedure
------------------------------

FW upgrades currently must be done with the serial RS232-1 port and can be done on Windows and Linux computers.

Please ensure power and serial is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, please power cycle the unit and try again.

1. Connect RS-232-1 to computer.
2. Download ANELLO-provided FW image onto your local computer
3. Open device manager, and find which COM port is the serial connection.
4. In terminal:

.. code-block:: python
    :caption: Terminal
    
        # cd into folder with anello_fw_uploader.py
        python anello_fw_uploader.py --port COM5 --baud-bootloader 115200 /Users/user1/Downloads/anello_maritime_default.anello
        # Change "COM5" to match your port that the Maritime INS is plugged into
        # Change "/Users/user1/Downloads/anello_maritime_default.anello" to the path to the ANELLO-provided FW image (.anello file) on your local computer

8. After it completes, you will see "Rebooting. Elapsed Time x.x" - this means the FW upgrade was successful

.. image:: media/FW_upgrade_success.png
   :width: 80%
   :align: center

*For support upgrading firmware with Linux, contact info@anellophotonics.com*