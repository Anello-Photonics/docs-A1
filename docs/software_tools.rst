Software Tools
=======================

ANELLO Python Program
------------------------
The ANELLO Python Program source code is found on the `ANELLO GitHub <https://github.com/Anello-Photonics/user_tool>`_. 

Please run "git pull" regularly to make sure you are using the latest code.

ROS Compatibility
---------------------------------
If you would like to integrate an ANELLO unit into ROS, see our C-based `ROS1 driver <https://github.com/Anello-Photonics/ANELLO_ROS_Driver>`_
or `ROS2 driver <https://github.com/Anello-Photonics/ANELLO_ROS_Driver/tree/ros2_main>`_

Please run "git pull" regularly to make sure you are using the latest code.

Serial Interface Software
---------------------------------
You are welcome to use a serial interface software, such as CoolTerm, to interface with ANELLO units.

Please ensure you use the correct baud rate and set Data Bits = 8, Stop Bits = 1, and Parity = None. See `Comminication & Messaging <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html>`_ 
for information on data and configuration ports and default baud rates.

Sample Odometer Programs
---------------------------------
The message format and instructions for sending odometer information to an ANELLO EVK or GNSS INS can be found under `Communication & Messaging <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html#apodo-message>`_.
We have also included several sample odometer programs in user_tool -> `board_tools <https://github.com/Anello-Photonics/user_tool/tree/main/board_tools>`_, supporting both UDP and serial interfaces.
