Firmware Upgrade
======================

ANELLO recommends maintaining the latest firmware for best results. The latest firmware release is v1.2.5. 
If you are on an older version, please contact ANELLO for the latest firmware and follow the instructions below to upgrade.

Firmware upgrades currently must be done on a Windows machine. Please ensure power is not disrupted to the unit during the firmware upgrade process. 
If you experience any errors during the process, power cycle the unit and try again.

.. note::
   Make sure to first run "git pull" to make sure you are using the latest user_tool functionality.

    1. Ensure HtxAurixBootLoader.exe is in user_tool -> board_tools

    2. Run user_program.py and connect to unit over COM (USB)
        
    3. On main menu, run Upgrade -> Enter and select ANELLO-provided .hex file
        1. Upgrade will run automatically and typically takes about 5 minutes to completed
        2. If you experience any issues, power cycle the unit and start again from step 2

    4. After successful upgrade, the FW version will be updated in System Status upon re-connecting to the unit