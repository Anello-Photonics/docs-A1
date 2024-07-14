Unit Configurations
=======================

The easiest way to configure an ANELLO unit is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/latest/python_tool.html#unit-configurations>`_, 
which saves all changes to non-volatile flash memory. 

Alternatively, the unit can be configured using the APCFG message, which allows for both temporary (RAM) and permanent setting (FLASH) of configuration parameters.

**#APCFG,<r/w/R/W>,<param1>,<value1>,...,<paramN>,<valueN>*checksum**

  +---+------------+-------------------------------------------------------------------------------------+
  |   | Field      |  Description                                                                        |
  +---+------------+-------------------------------------------------------------------------------------+
  | 0 | APCFG      |  Sentence identifier                                                                |
  +---+------------+-------------------------------------------------------------------------------------+
  | 1 |<read/write>|  'r': read  RAM, 'w': write RAM, 'R': read FLASH, 'W': write FLASH                  |
  +---+------------+-------------------------------------------------------------------------------------+
  | 2 | <param>    |  Configuration parameter (APCFG code)                                               |
  +---+------------+-------------------------------------------------------------------------------------+
  | 3 | <value>    |  Configuration value, expressed in ASCII                                            |
  +---+------------+-------------------------------------------------------------------------------------+
  | 4 | checksum   |  XOR of bytes between # and \* written in hexadecimal (letters must be uppercase)   |
  +---+------------+-------------------------------------------------------------------------------------+

Unit Configuration Settings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The available parameters and values to configure are described in the table below:

  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Configuration          | APCFG Code | Value/Description                                                                                   |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Orientation            | orn        | Coordinate axes for mounting position, default +X+Y+Z (see below)                                   |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Alignment Angles       | aln        | Alignment angles of unit in degrees, in +X+Y+Z (roll, pitch, yaw) order. Default +0.0+0.0+0.0       |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Output Data Rate       | odr        | Output rate of APIMU message: 20, 50, 100, or 200 Hz. Requires reset.                               |
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
  | APCFG Output           | min        | Minutes between output of APCFG configuration values over the data port (0 disables output)         |
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
  | Output Message Format  | mfm        | Format of the output messages. 1: ASCII, 4: RTCM (default)                                          |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable Serial Output   | uart       | Enable output over serial interface: 'on', 'off'                                                    |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Enable Ethernet Output | eth        | Enable output over ethernet interface: 'on', 'off'                                                  |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | NMEA Output Messages   | nmea       | Configures up to 8 NMEA messages to be output on the config port (0-255). 0: off, 255: all on       |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | Baud Rate              | bau        | Serial communication baud rate in bits per second. Requires reset.                                  |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+
  | NHC                    | nhc        | NHC functionality in EKF. Valid values range from 0 to 7. 0 – default car case, 1 – truck case, 2 – ag case, 7 – drone case (NHC off)    |
  +------------------------+------------+-----------------------------------------------------------------------------------------------------+

.. note:: Some configurations require a system reset after changing, such as the ODR and baud rate. This can be done by selecting "Reset" in the user_program.py main menu, or sending the reset command over the Configuration port: #APRST,0*58 

.. note:: The UDP ports are the numbers on the connected computer only. The EVK uses UDP port 1 for data, 2 for configuration, and 3 for odometer.

Output Data Rate (ODR)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The GNSS INS has output data rate constraints when outputting data over RS-232. In RTCM or binary messaging mode, maximum ODR is 100 Hz. In ASCII mode, maximum ODR is 50 Hz.
All other ANELLO units support ODR up to 200 Hz. RTCM message format is recommended for best timing.

.. note:: Decreasing the baud rate will affect the maximum output data rate. It is recommended to keep the default baud rate (921600 for EVK; 230400 for GNSS INS and IMU) enable highest ODR.

Unit Installation Orientation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Orientation describes the mounting orientation of the ANELLO Unit on the vehicle. 
This configuration is only used in the ANELLO algorithm and does not affect IMU data output.

The following 8 right hand rule frames are possible:

    1. +X+Y+Z  Default; Unit mounted upright with X pointing in vehicle forward
    2. +Y-X+Z  Unit mounted upright with X pointing in vehicle right
    3. -Y+X+Z  Unit mounted upright with X pointing in vehicle left
    4. -X-Y+Z  Unit mounted upright with X pointing in vehicle back
    5. +X-Y-Z  Unit mounted upside down with X pointing in vehicle forward
    6. +Y+X-Z  Unit mounted upside down with X pointing in vehicle right
    7. -Y-X-Z  Unit mounted upside down with X pointing in vehicle left
    8. -X+Y-Z  Unit mounted upside down with X pointing in vehicle back