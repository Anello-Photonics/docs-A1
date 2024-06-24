======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is v1.3.24 (released June 17, 2024). 
If you are on an older version, please contact ANELLO for the latest FW image and release notes.

Firmware Upgrade Procedure
--------------------------------------
FW upgrades currently must be done on a Windows machine. Please ensure power is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, power cycle the unit and try again.

.. note::
   Make sure to first run "git pull" to make sure you are using the latest user_tool functionality.

    1. Ensure HtxAurixBootLoader.exe is in user_tool -> board_tools

    2. Run user_program.py and connect to unit over COM (USB)
        
    3. On main menu, run Upgrade -> Enter and select ANELLO-provided .hex file
        1. Upgrade will run automatically and typically takes about 5 minutes to completed
        2. If you experience any issues, power cycle the unit and start again from step 2

    4. After successful upgrade, the FW version will be updated in System Status upon re-connecting to the unit


Firmware Upgrade Notes
------------------------------
Please review below for key considerations when upgrading between different FW versions. 
Always make sure to run "git pull" on user_tool to make sure you have all the latest configurations and calibration schemes.
Please contact ANELLO for the latest firmware image and full release notes.

Upgrading from v1.2 to v1.3
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you are upgrading from v1.2, a new ZUPT calibration is available to take advantage of improved state estimates while stationary.

Upgrading from v1.1
~~~~~~~~~~~~~~~~~~~~~~~
If you are upgrading from v1.1 or older:

1. Completing antenna baseline calibration is now required to use the dual antenna feature.
2. An installation misalignment angle configuration is now available, which improves performance on long-distance dead reckoning.

Upgrading from v1.0 and older
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you are upgrading from v1.0 or older, there are several new “Unit Configurations” which will
default to OFF when you upgrade the firmware. Please make sure they are set properly upon upgrading firmware:

1. Enable Serial Output: defaults OFF, turn ON if outputting over serial.
2. Enable Ethernet Output: defaults OFF, turn ON if outputting over ethernet.
3. NTRIP Input Channel: defaults OFF (no NTRIP data used), set to Serial if sending NTRIP over serial, set to Ethernet if sending NTRIP over ethernet.
4. Sensor low-pass filter cutoff frequencies are defaulted to 0 (no filter). We recommend setting these to 90% Nyquist (90 Hz for output data rate of 200 Hz).