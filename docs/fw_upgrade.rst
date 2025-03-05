======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is:

- GNSS INS, EVK, IMU+: v1.3.24 (released June 17, 2024)
- X3: v1.0.8 (Released Dec 13, 2024)

If you are on an older version, please contact ANELLO for the latest FW image.

FW upgrades currently must be done over the serial interface and can be done on computers using the following OS/processors:

- GNSS INS, EVK, X3: Windows, Linux (x86), Linux (ARM)
- IMU+: Windows only

Please ensure power and serial connection is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, please power cycle the unit and try again.

Firmware Upgrade Procedure - Python Tool
------------------------------------------
Make sure to first run "git pull" in user_tool to ensure you are using the latest firmware upgrade functionality.

    1. Connect both serial ports to a Windows, Linux (x86), or Linux (ARM) computer using the provided USB cable (EVK) or DB9 to USB cables (all other units).
    
    2. Run user_program.py (for EVK, GNSS INS, and IMU+) or x3_tool.py (for X3) and connect to unit over COM (USB)
        
    3. On main menu, select Upgrade -> Yes. Select ANELLO-provided .hex file
        - Upgrade will run automatically and typically takes about 5 minutes to completed
        - If you experience any issues, power cycle the unit and start again from step 2

    3. After successful upgrade, the FW version will be updated in System Status upon re-connecting to the unit

Firmware Upgrade Procedure - Command Line
------------------------------------------
Connect both serial ports to a Windows, Linux (x86), or Linux (ARM) computer using the provided USB cable (EVK) or DB9 to USB cables (all other units).

To enter bootloading mode, send the following command to the UART port using a serial interface program such as CoolTerm:
#APRST,2*5A

In a terminal, navigate to the bootloader (found in user_tool -> board_tools directory) and locate the correct bootloader for your OS (using Windows x86 as an exampl below).
Enter the following commands one at a time:

    1. ./crossplatform_bootloader_windows_x86_release START TC36X 6 <RS-422 port #> 115200 0 0 0 0
        a. If the X3 RS-422 port is COM8, you would enter 8 for <RS-422 port #>
    2. ./crossplatform_bootloader_windows_x86_release PROGRAM <hex file path>
    3. ./crossplatform_bootloader_windows_x86_release END

After each step it should show "Operation Successful!" If it doesn't, repeat the last step.

EVK / GNSS INS Firmware Upgrade Notes
---------------------------------------
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