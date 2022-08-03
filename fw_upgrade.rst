Firmware Upgrade
======================

To update the firmware version using an ANELLO-provided .hex file:
    1. Make sure HtxAurixBootLoader.exe in user_tool -> board_tools

    2. Add new .hex file to user_tool -> board_tools

    3. Run user_program.py
        1. Connect -> COM, take note of data port COM # in System Status
        2. Main menu -> Upgrade -> Enter
        3. Exit user_program.py

    4. In terminal, enter the following commands, with the correct data port # (e.g. 6 for COM6 in step 3.1) and hex file name (including .hex):
        
       HtxAurixBootLoader START TC36X 6 <data port number> 115200 0 0 0 0
       HtxAurixBootLoader PROGRAMVERIFY <hex file name> 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFF0000 0x0
       HtxAurixBootLoader END

       After each step it should show "operation successful". If it doesn't, repeat the last step. If that doesn't work, unplug USB and start over from step #3.

    5. FW version will be updated in System Status upon re-connecting to EVK