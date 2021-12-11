Unit Configurations
=======================

Unit configurations control operation of the A1. They can be set with the Anello Python Program by selecting "UNIT CONFIGURATIONS" in the main menu.
They can also be set by **#APCFG** messages over the usb or ethernet connection.


+------------------------+-------------------+----------------------------------------------------------------------+
| Configuration          |  Code (for APCFG) | Value / Description                                                  |
+------------------------+-------------------+----------------------------------------------------------------------+
| Orientation            |        orn        |   Coordinate axes for outputs, e.g. **+X-Y-Z** - see below           |
+------------------------+-------------------+----------------------------------------------------------------------+
| Output Data Rate       |        odr        |  rate of APIMU messages:  **20,50,100,200** Hz                       |
+------------------------+-------------------+----------------------------------------------------------------------+
| Enable GPS 1           |        gps1       |  use the first gps antenna: "on", "off"                              |
+------------------------+-------------------+----------------------------------------------------------------------+
| Enable GPS 2           |        gps2       |  use the second gps antenna: "on", "off"                             |
+------------------------+-------------------+----------------------------------------------------------------------+
| Odometer Unit          |        odo        |  Speed unit of the odometer message: "mps", "mph", "kph", "fps"      |
+------------------------+-------------------+----------------------------------------------------------------------+
| DHCP (auto assign IP)  |        dhcp       |  DHCP "on", "off" : let router assign A-1 IP for udp messaging.      |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP A1 IP              |        lip        |  A-1's IP address, applies only if DHCP off:  (aaa.bbb.ccc.ddd)      |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP COMPUTER IP        |        rip        |  IP address of computer to for UDP communication: (aaa.bbb.ccc.ddd)  |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP DATA PORT          |       rport1      |  computer's udp port for data output: integer from 1 to 65535        |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP CONFIGURATION PORT |       rport2      |  computer's udp port for config messaging: integer from 1 to 65535   |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP ODOMETER PORT      |       rport3      |  computer's udp port for odometer messaging: integer from 1 to 65535 |
+------------------------+-------------------+----------------------------------------------------------------------+

.. note::
    Orientation describes the coordinate axes used in IMU and INS output in terms of the A1 coordinate axes (shown on A1 label)

    The first pair of symbols represents the x axis of the new frame in terms of the original A1 coordinate axes.
    eg: -Y means that the new x axis is the negation of the original y axis. The second and third pairs describe the new y and z axes.

    These values are possible: the 8 right-handed frames with Z mapped to +-Z

    1. +X+Y+Z 	  default, matches coordinate axes on A1 label
    2. +Y+X-Z
    3. -X-Y+Z
    4. +Y-X+Z
    5. -Y+X+Z
    6. +X-Y-Z
    7. -X+Y-Z
    8. -Y-X-Z

    This affects the accelerations and rates in the APIMU message.
    Suppose the unit in in the default +X+Y+Z shows an acceleration vector of <i, j, k>.
    Then in the in the Y-X+Z frame, the APIMU message would show accelerations <j,-i,k>.