======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is v1.3.24 (released June 17, 2024). 
If you are on an older version, please contact ANELLO for the latest FW image and release notes.

Firmware Upgrade Procedure
--------------------------------------
FW upgrades currently must be done over the serial interface and can be done on computers using the following OS/processors:

1. GNSS INS, EVK, X3: Windows, Linux (x86), Linux (ARM)
2. IMU+: Windows only

Please ensure power and serial connection is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, please power cycle the unit and try again.
Make sure to first run "git pull" in user_tool to ensure you are using the latest firmware upgrade functionality.

    1. Ensure HtxAurixBootLoader.exe is in user_tool -> board_tools

    2. Run user_program.py and connect to unit over COM (USB)
        
    3. On main menu, select Upgrade -> Yes and select ANELLO-provided .hex file
        1. Upgrade will run automatically and typically takes about 5 minutes to completed
        2. If you experience any issues, power cycle the unit and start again from step 2

    4. After successful upgrade, the FW version will be updated in System Status upon re-connecting to the unit


Firmware Upgrade Notes
------------------------------
Please review the key considerations below to ensure the best performance when upgrading between different FW versions. 
Always make sure to run "git pull" to make sure your user_tool has all the latest configurations and calibration schemes.
Please contact ANELLO for the latest firmware image and full release notes.

Upgrading from v1.2
~~~~~~~~~~~~~~~~~~~~~~~
If you are upgrading from v1.2, improvements in were made in v1.3 during stationary periods, particularly when odometer input is not provided.
To take advantage of these improvements, a `ZUPT calibration <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html#zupt-calibration>`_ must be performed.

Upgrading from v1.1
~~~~~~~~~~~~~~~~~~~~~~~
If you are upgrading from v1.1, in addition to the items above:

1. Completing `antenna baseline calibration <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html#dual-antenna-baseline-calibration>`_ is now required to use the dual antenna feature.
2. An installation `misalignment angle configuration <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html#anello-unit-installation-misalignment>`_ is now available, which improves performance on long-distance dead reckoning.

Upgrading from v1.0
~~~~~~~~~~~~~~~~~~~~~~~
If you are upgrading from v1.0 or older, in addition to the items above:

The IMU message now has an additional field (T_Sync) which wasn't included in previous firmwares.

There are also several new `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_ which will default to OFF when upgrading from v1.0 or earlier.
Please make sure these are set properly upon upgrading:

1. Enable Serial Output: defaults OFF, turn ON if outputting over serial.
2. Enable Ethernet Output: defaults OFF, turn ON if outputting over ethernet.
3. NTRIP Input Channel: defaults OFF (no NTRIP data used), set to Serial if sending NTRIP over serial, set to Ethernet if sending NTRIP over ethernet.
4. Sensor low-pass filter cutoff frequencies are defaulted to 0 (no filter). We recommend setting these to 90% Nyquist (90 Hz for output data rate of 200 Hz).

Legacy Units
~~~~~~~~~~~~~~~~~
Units received before Nov 1, 2022 are an older hardware revision that are not dual GNSS capable and may be limited in performance. 
These older units are are not guaranteed compatibility with newer software and features. 
To upgrade to the latest hardware, please contact ANELLO.