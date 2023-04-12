Unit Configurations
=======================
To adjust configuration, select *Unit Configuration* from the main menu.
To change a configuration, select *Edit* and then the configuration to change. Select or type in the new value.

General configurations:  

- Orientation coordinate system for EVK (+X+Y+Z or other right handed frames). See bottom of page.  
- Output Data Rate (20/50/100/200 Hz)
- Rate of INS message output in Hz. Must be divisible by the ODR
- Enable GPS (on/off) for each reciever 
- Odometer Units (mps/mph/kph/fps) 
- Enable FOG (on/off)

UDP connection configurations:  

- DHCP (on/off): If on, the EVK IP is assigned by router. If off, pick the IP yourself.  
- UDP A-1 IP: IP address for the EVK. Can only set this when DHCP is off  
- UDP computer IP: IP address of your computer, which EVK will connect to.  
- UDP computer data port: Remote device's port for data channel. This works like the data and config COM ports.  
- UDP computer configuration port: Remote devices port for config channel 

.. note:: The above UDP ports are the numbers on the connected computer only. The EVK uses UDP port 1 for data and 2 for configuration. 

.. note:: If sending odometer speeds by UDP from another program, send to UDP port 2 on the EVK, from the computer UDP config port matching the configurations. 
|

  +------------------------+------------+-----------------------------------------------------------------------+
  | Configuration          | APCFG Code | Value/Description                                                     |
  +------------------------+------------+-----------------------------------------------------------------------+
  | Orientation            | orn        | Coordinate axes for outputs, e.g. +X-Y-Z (see below)                  |
  +------------------------+------------+-----------------------------------------------------------------------+
  | Output Data Rate       | odr        | Rate of APIMU messages: 20, 50, 100, or 200 Hz                        |
  +------------------------+------------+-----------------------------------------------------------------------+
  | Enable GPS 1           | gps1       | Use primary antenna (ANT1): 'on', 'off'                               |
  +------------------------+------------+-----------------------------------------------------------------------+
  | Enable GPS 2           | gps2       | Use secondary antenna (ANT2): 'on', 'off'                             |
  +------------------------+------------+-----------------------------------------------------------------------+
  | Odometer Units         | odo        | Speed unit of the odometer message: 'mps', 'mph', 'kph', 'fps'        |
  +------------------------+------------+-----------------------------------------------------------------------+
  | DHCP (auto-assign IP)  | dhcp       | DHCP (let router assign EVK IP for UDP messaging): 'on', 'off'        |
  +------------------------+------------+-----------------------------------------------------------------------+
  | EVK UDP Port           | lip        | EVK's IP address, applies only if DHCP off: (aaa.bbb.ccc.ddd)         |
  +------------------------+------------+-----------------------------------------------------------------------+
  | Computer UDP Port      | rip        | IP address of computer for UDP communication: (aaa.bbb.ccc.eee)       |
  +------------------------+------------+-----------------------------------------------------------------------+
  | UDP Data Port          | rport1     | Computer's UDP port for data output: integer from 1 to 65535          |
  +------------------------+------------+-----------------------------------------------------------------------+
  | UDP Configuration Port | rport2     | Computer's UDP port for config messaging: integer from 1 to 65535     |
  +------------------------+------------+-----------------------------------------------------------------------+
  | UDP Odometer Port      | rport3     | Computer's UDP port for odometer messaging: integer from 1 to 65535   |
  +------------------------+------------+-----------------------------------------------------------------------+

Orientation describes the coordinate axes used in IMU and INS output in terms of the EVK coordinate axes (shown on EVK label).

    The first pair of symbols represents the X axis of the new frame in terms of the original EVK coordinate axes.
    eg: -Y means that the new X axis is the negation of the original Y axis. The second and third pairs describe the new Y and Z axes.

The following 8 right-handed frames are possible:

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