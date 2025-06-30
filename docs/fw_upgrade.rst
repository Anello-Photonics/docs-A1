======================
Firmware Upgrade
======================

ANELLO recommends using the latest firmware (FW) for best results. The latest FW release is:

- v1.3.24 (released June 17, 2024)


If you are on an older version, please contact ANELLO for the latest FW image.

FW upgrades currently must be done over the serial interface and can be done on computers using the following OS/processors:
- Windows only

Please ensure power and serial connection is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, please power cycle the unit and try again.

Firmware Upgrade Procedure - Python Tool
------------------------------------------
Make sure to first run "git pull" in user_tool to ensure you are using the latest firmware upgrade functionality.

    1. Connect both serial ports to a Windows computer using the provided USB cable (EVK) or DB9 to USB cables (all other units).
    
    2. Run user_program.py and connect to unit over COM (USB)
        
    3. On main menu, select Upgrade -> Yes. Select ANELLO-provided .hex file
        - Upgrade will run automatically and typically takes about 5 minutes to completed
        - If you experience any issues, power cycle the unit and start again from step 2

    3. After successful upgrade, the FW version will be updated in System Status upon re-connecting to the unit

If for any reason the power or serial connection gets disrupted during the firmware upgrade process and power cycling doesn't bring back the unit to a 
functioning state, please try running the bootloader commands from the command line as described below (starting with step 1).

Firmware Upgrade Procedure - Command Line
------------------------------------------
Connect both serial ports to a Windows computer using the provided USB cable (EVK) or DB9 to USB cables (all other units).

To enter bootloading mode, send the following command to the UART port using a serial interface program such as CoolTerm:
#APRST,2*5A

In a terminal, navigate to the bootloader (found in user_tool -> board_tools directory) and locate the correct bootloader for your OS (using Windows x86 as an exampl below).
Enter the following commands one at a time:

    1. ./crossplatform_bootloader_windows_x86_release START TC36X 6 <data port #> 115200 0 0 0 0
        a. E.g. if the data port is COM8, you would enter 8 for <data port #>
    2. ./crossplatform_bootloader_windows_x86_release PROGRAM <hex file path>
    3. ./crossplatform_bootloader_windows_x86_release END

After each step it should show "Operation Successful!" If it doesn't, repeat the last step.
