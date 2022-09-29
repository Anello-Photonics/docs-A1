Unit Configurations
=======================
To adjust configuration, select *Configure* from the main menu while connected. The current configurations will display.
To change a configuration, select *Edit* and then the configuration to change. Select or type in the new value.

General configurations:
-   Output Data Rate    (20/50/100/200) - rate of INS and IMU message outputs in Hz
-   Orientation         (+X+Y+Z or other right handed frames) - coordinate system for EVK
-   Enable GPS          (on/off) - let the EVK use the GPS antenna
-   Odometer Unit       (mps/mph/kph/fps) - speed unit for odometer input
-   Enable FOG          (on/off) - let the EVK use the optical gyro for angular rate z

UDP connection configurations:
-   DHCP (on/off):               If on, the EVK ip is assigned by router. If off, pick the ip yourself.
-   UDP A-1 IP:                       IP address for the EVK. Can only set this when DHCP is off
-   UDP computer IP:                   IP address of your computer, which EVK will connect to.
-   UDP computer data port:            Remote device's port for data channel. This works like the data and config com ports.
-   UDP computer configuration port:   Remote devices port for config channel

.. note::
    The above UDP ports are the numbers on the connected computer only. The EVK uses UDP ports 1 for data and 2 for configuration.
    If sending odometer speeds by UDP from another program, send to UDP port 2 on the EVK, from the computer UDP config port matching the configurations.


Unit configurations can also be set by **#APCFG** messages over the usb or ethernet connection.


+------------------------+-------------------+----------------------------------------------------------------------+
| Configuration          |  Code (for APCFG) | Value / Description                                                  |
+------------------------+-------------------+----------------------------------------------------------------------+
| Orientation            |        orn        |  Coordinate axes for outputs, e.g. **+X-Y-Z** - see below            |
+------------------------+-------------------+----------------------------------------------------------------------+
| Output Data Rate       |        odr        |  Rate of APIMU messages:  **20,50,100,200** Hz                       |
+------------------------+-------------------+----------------------------------------------------------------------+
| Enable GPS 1           |        gps1       |  Use the first gps antenna: "on", "off"                              |
+------------------------+-------------------+----------------------------------------------------------------------+
| Enable GPS 2           |        gps2       |  Use the second gps antenna: "on", "off"                             |
+------------------------+-------------------+----------------------------------------------------------------------+
| Odometer Unit          |        odo        |  Speed unit of the odometer message: "mps", "mph", "kph", "fps"      |
+------------------------+-------------------+----------------------------------------------------------------------+
| DHCP (auto assign IP)  |        dhcp       |  DHCP "on", "off" : let router assign EVK IP for UDP messaging.      |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP A1 IP              |        lip        |  EVK's IP address, applies only if DHCP off:  (aaa.bbb.ccc.ddd)      |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP COMPUTER IP        |        rip        |  IP address of computer to for UDP communication: (aaa.bbb.ccc.ddd)  |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP DATA PORT          |       rport1      |  Computer's UDP port for data output: integer from 1 to 65535        |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP CONFIGURATION PORT |       rport2      |  Computer's UDP port for config messaging: integer from 1 to 65535   |
+------------------------+-------------------+----------------------------------------------------------------------+
| UDP ODOMETER PORT      |       rport3      |  Computer's UDP port for odometer messaging: integer from 1 to 65535 |
+------------------------+-------------------+----------------------------------------------------------------------+

.. note::
    Orientation describes the coordinate axes used in IMU and INS output in terms of the EVK coordinate axes (shown on EVK label)

    The first pair of symbols represents the X axis of the new frame in terms of the original EVK coordinate axes.
    eg: -Y means that the new X axis is the negation of the original Y axis. The second and third pairs describe the new Y and Z axes.

    These values are possible: the 8 right-handed frames with Z mapped to +-Z

    1. +X+Y+Z 	  Default, matches coordinate axes on EVK label
    2. +Y+X-Z
    3. -X-Y+Z
    4. +Y-X+Z
    5. -Y+X+Z
    6. +X-Y-Z
    7. -X+Y-Z
    8. -Y-X-Z

    If the unit is in the default +X+Y+Z, the acceleration vector would be <i, j, k>.
    In the Y-X+Z frame, the APIMU message would show accelerations <j, -i, k>.