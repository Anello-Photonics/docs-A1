Unit Configurations
=======================

The easiest way to configure the EVK is with the ANELLO Python Program, which saves all changes to non-volatile flash memory. 
To do this, see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_.

Alternatively, the EVK can be dynamically configured using the APCFG message. The protocol allows for both temporary (RAM) and permanent setting (FLASH) of configuration parameters.

**#APCFG,<r/w/R/W>,<param>,<value1>,..,<valueN>*checksum**

  +---+------------+-----------------------------------------------------------------------+
  |   | Field      |  Description                                                          |
  +---+------------+-----------------------------------------------------------------------+
  | 0 | APCFG      |  Sentence identifier                                                  |
  +---+------------+-----------------------------------------------------------------------+
  | 1 |<read/write>|  'r': read  RAM, 'w': write RAM, 'R': read FLASH, 'W': write FLASH    |
  +---+------------+-----------------------------------------------------------------------+
  | 2 | <param>    |  Configuration parameter (APCFG code)                                 |
  +---+------------+-----------------------------------------------------------------------+
  | 3 | <value>    |  Configuration value, expressed in ASCII                              |
  +---+------------+-----------------------------------------------------------------------+

The available parameters and values to configure are described in the table below:

  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Configuration          | APCFG Code | Value/Description                                                                                   |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Orientation            | orn        | Coordinate axes for outputs, e.g. +X+Y+Z (see below)                                                |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Output Data Rate       | odr        | Rate of APIMU messages: 20, 50, 100, or 200 Hz. Requires reset.                                     |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable GPS 1           | gps1       | Use primary antenna (ANT1): 'on', 'off'                                                             |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable GPS 2           | gps2       | Use secondary antenna (ANT2): 'on', 'off'                                                           |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Odometer Units         | odo        | Speed unit of the odometer message: 'mps', 'mph', 'kph', 'fps'                                      |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable FOG             | fog        | Enable optical gyro data to be used in algorithm: 'on', 'off'                                       |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | DHCP (auto-assign IP)  | dhcp       | DHCP (let router assign EVK IP for UDP messaging): 'on', 'off'                                      |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | EVK UDP Port           | lip        | EVK's IP address, applies only if DHCP off: aaa.bbb.ccc.ddd                                         |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Computer UDP Port      | rip        | IP address of computer for UDP communication: aaa.bbb.ccc.eee                                       |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | UDP Data Port          | rport1     | Computer's UDP port for data output (works like data serial port): integer from 1 to 65535          |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | UDP Configuration Port | rport2     | Computer's UDP port for config messaging (works like data serial port): integer from 1 to 65535     |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | UDP Odometer Port      | rport3     | Computer's UDP port for odometer messaging: integer from 1 to 65535                                 |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | APCFG Output           | min        | Minutes between output of APCFG configuration values over the data port (0 disables the output)     |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Accel Cutoff Freq      | lpa        | Low-pass filter cutoff frequency [Hz] for the MEMS accelerometer (0 disables filter)                |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | MEMS Gyro Cutoff Freq  | lpw        | Low-pass filter cutoff frequency [Hz] for the MEMS angular rate sensor (0 disables filter)          |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | FOG Cutoff Freq        | lpo        | Low-pass filter cutoff frequency [Hz] for the optical gyro (0 disables filter)                      |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | NTRIP Input Channel    | ntrip      | Input channel for NTRIP data. 0: off (default), 1: Serial, 2: UDP                                   |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Sync Pulse Enable      | sync       | Enables the external synchronization pulse input: 'on', 'off'                                       |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Output Message Format  | mfm        | Format of the output messages. 1: ASCII, 4: RTCM                                                    |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable Serial Output   | uart       | Enable output over serial interface: 'on', 'off'                                                    |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable Ethernet Output | eth        | Enable output over ethernet interface: 'on', 'off'                                                  |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | NMEA Output Messages   | nmea       | Format of the output messages. 1: ASCII, 4: RTCM                                                    |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Output Message Format  | mfm        | Configures up to 8 NMEA messages to be output on the config port (0-255). 0: off, 255: all on       |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Baud Rate              | bau        | Serial communication baud rate in bits per second. Requires reset.                                  |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+


.. note:: The GNSS INS unit has output data rate constraints when outputting data over RS-232. In RTCM mode, ODR is limited to 100 Hz. In ASCII mode, ODR is limited to 50 Hz.

.. note:: The above UDP ports are the numbers on the connected computer only. The EVK uses UDP port 1 for data, 2 for configuration, 3 for odometer.

.. note:: If sending odometer speeds by UDP from another program, send to UDP port 3 on the EVK, from the computer's UDP port matching "odometer port" configuration.


Some configurations require a system reset after changing, such as the ODR and baud rate: #APRST,0*58 

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