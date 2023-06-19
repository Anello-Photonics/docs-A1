Python Program
=======================

The ANELLO Python Program source code is found here: `ANELLO GitHub <https://github.com/Anello-Photonics/user_tool>`_

Please run "git pull" regularly to make sure you are using the latest code.


2   Sample Odometer Programs
---------------------------------
The message format and instructions for sending odometer information can be found under `Communication & Messaging <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html#apodo-message>`_.
We have also included several sample odometer programs in the board_tools directory of user_tool, supporting both UDP and serial interfaces.


2   ROS Compatibility
---------------------------------
For compatibility with ROS, please use the branch of our public Python tool called `ros_driver <https://github.com/Anello-Photonics/user_tool/tree/ros_driver>`_, 
which makes our python program an API that’s pip installable. For reference, the files containing the main functionality are located `here <https://github.com/Anello-Photonics/user_tool/tree/ros_driver/board_tools/src/anello_tools>`_. 

For typical implementations involving connecting to the EVK and reading messages, an example of the steps to implement this are as follows:
 
1. b = IMUBoard.auto to initialize through USB to get IP address and port numbers
    a. This could also be achieved simply by running our existing user_program.py and connecting over USB to get IP address and port numbers.
2. b = IMUBoard.from_udp to connect via UDP
    a. This requires the IP and port numbers from above.
3. m = b.read_one_message returns a message structure which contains data depending on the message read (e.g. IMU, GPS, INS)
4. Data can now be accessed, e.g.: m.gps_time_ns, m.lat_deg, etc (field names are defined in config -> readable_scheme_config.py)