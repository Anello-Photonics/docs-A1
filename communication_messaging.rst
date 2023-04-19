Communication & Messaging
===========================

1.  Port Definitions
---------------------

The Anello EVK communicates by serial interface (by USB cable) or UDP (over ethernet). The same port and message definitions apply for either interface.

The primary output port is known as the "Data" port, where the major output messages are transmitted at a configurable fixed output rate.
This port also serves as the input port for the RTCM correction stream.

The "Configuration" port is used for configuration messaging, and also serves as the input port for odometer aiding messages.

For UDP only, there is a third "Odometer" port to receive odometer messages, which is preferred to keep the configuration port free.
Over serial, use the configuration port for odometer messages.

UDP communication uses fixed port numbers on the EVK but selactable ports on the external device.
These ports, along with IP addresses and other UDP settings should be configured in user_program.py (see the Unit Configurations section).

Serial communication occurs at a baudrate of 921600 bits per second for the EVK product and 230400 for GNSS/INS and IMU products.

    +--------------------+------------------------------------------+---------------------------------------+
    | **Logical Port**   |  **Physical Port**                       |  **Functions**                        |
    +--------------------+------------------------------------------+---------------------------------------+
    | Data Port          | Lowest serial port #, e.g. COM7          | Output Data Messages, Input RTCM Data |
    |                    +------------------------------------------+                                       |
    |                    | UDP: EVK port 1, computer port selectable|                                       |
    +--------------------+------------------------------------------+---------------------------------------+
    | Configuration Port | Highest serial port #, e.g. COM10        | Odometer Aiding, Configuration        |
    |                    +------------------------------------------+                                       |
    |                    | UDP: EVK port 2, computer port selectable|                                       |
    +--------------------+------------------------------------------+---------------------------------------+
    | Odometer Port      | UDP: EVK port 3, computer port selectable| Preferred Odometer Channel, UDP only  |
    +--------------------+------------------------------------------+---------------------------------------+
     

2.  ASCII Data Output Messages
-------------------------------

The ANELLO EVK has two message formats, the first is ASCII. The structures of all ASCII messages use the 
following conventions:

-	The lead code identifier for each record is '#'.
-	Each log or command is of variable length depending on amount of data and formats.
-	All data fields are delimited by a comma ',' with two exceptions:
- Each log ends with a hexadecimal checksum preceded by an asterisk and followed by a line termination using the carriage return and line feed characters.  
- The checksum is simple, just an XOR of all the bytes between # and \*, written in hexadecimal.
- APIMU and APINS messages are transmitted at the output data rate (ODR) setting. 
- APGPS messages are transmitted at the rate which the EVK receives messages internally. Default is 4 Hz. 


2.1. APIMU Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APIMU      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | T_Sync     |  ms       |  Time at last sync rising edge (zero when sync disabled)              |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 3 | AX         |  g        |  X-Axis Acceleration                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 4 | AY         |  g        |  Y-Axis Acceleration                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 5 | AZ         |  g        |  Z-Axis Acceleration                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 6 | WX         |  deg/s    |  X-Axis Angular Rate (MEMS)                                           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 7 | WY         |  deg/s    |  Y-Axis Angular Rate (MEMS)                                           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 8 | WZ         |  deg/s    |  Z-Axis Angular Rate (MEMS)                                           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 9 | OG_WZ      |  deg/s    |  High Precicision Z-Axis Angular Rate (ANELLO Optical Gyro)           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 10| ODO        |  m/s      |  Scaled Composite Odometer Value                                      |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 11| ODO Time   |  ms       |  Timestampe of Odometer Reading                                       |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 12| Temp C     |  °C       |  Temperature                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  
.. note:: 
  Firmware before v0.2.1 also has the Optical Gyro voltage MEMS Z rate and Optical Gyro rate.

  Firmware before v1.0.39 does not have T_Sync field.


2.2. APGPS Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  +---+---------------+-----------+-----------------------------------------------------------------------+
  |   | Field         |  Units    |  Description                                                          |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 0 | APGPS         |           |  Sentence identifier                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time          |  ms       |  Time since power on                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 2 | GPS Time      |  ns       |  GPS Time (not UTC time)                                              |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 3 | Lat           |  deg      |  Latitude, '+': north, '-': south                                     |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 4 | Long          |  deg      |  Longitude, '+': east, '-': west                                      |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 5 | Alt ellipsoid |  m        |  Height above ellipsoid                                               |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 6 | Alt msl       |  m        |  Height above mean sea level                                          |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 7 | Speed         |  m/s      |  Speed                                                                |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 8 | Heading       |  deg      |  GNSS Heading (ground track)                                          |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 9 | Hacc          |  m        |  Horizontal Accuracy                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 10| Vacc          |  m        |  Vertical Accuracy                                                    |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 11| PDOP          |           |  Position dilution of precision                                       |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 12| FixType       |           |  0: No Fix, 2: 2D Fix, 3: 3D Fix, 5: Time Only                        |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 13| SatNum        |           |  Number of satellites used in solution                                |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 14| Speed Acc     |           |  Accuracy of GNSS speed measurement                                   |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 15| Hdg Acc       |           |  Accuracy of GNSS heading measurement                                 |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 16| RTK Status    |           |  0: Single Point Positioning, 1: RTK Float, 2: RTK Fixed              |
  +---+---------------+-----------+-----------------------------------------------------------------------+

.. note:: FW v1.0.0 and later: This packet should be used to correlate GPS time and system time. The packet is time stamped at the time the PPS signal is generated by the GNSS receiver.


2.3. APINS Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                                                                            |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 0 | APINS      |           |  Sentence identifier                                                                                                    |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                                                                    |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 2 | GPS Time   |  ns       |  GPS Time in integer ns                                                                                                 |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 3 | Status     |           |  0: Attitude Only, 1: Position and Attitude, 2: Position, Attitude, and Heading, 3: RTK Float, 4: RTK Fixed             |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 4 | Lat        |  deg      |  Latitude, '+': north, '-': south                                                                                       |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 5 | Long       |  deg      |  Longitude, '+': east, '-': west                                                                                        |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 6 | Height     |  m        |  Height above ellipsoid                                                                                                 |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 7 | VN         |  m/s      |  North Velocity in NED Frame                                                                                            |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 8 | VE         |  m/s      |  East Velocity in NED Frame                                                                                             |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 9 | VD         |  m/s      |  Down Velocity in NED Frame                                                                                             |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 10| Roll       |  deg      |  Roll Angle, rotation about body frame X                                                                                |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 11| Pitch      |  deg      |  Pitch Angle, rotation about body frame Y                                                                               |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 12| Heading    |  deg      |  Heading Angle, rotation about body frame Z                                                                             |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+
  | 13| ZUPT       |           |  0: Moving, 1: Stationary                                                                                               |
  +---+------------+-----------+-------------------------------------------------------------------------------------------------------------------------+

.. note:: GPS time in this packet is interpolated from the last GNSS fix and system time. 

.. note:: Roll, pitch and heading angles are calculated as standard aerospace Euler angles.


3.  Binary Data Output Messages
----------------------------------

3.1. Message format
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The binary packets use an RTCM standard 10403 envelope for each message. 

  +---+-----------+--------------------------------------------------------------+
  |   | Field     |  Value/Description                                           |
  +---+-----------+--------------------------------------------------------------+
  | 0 | Preamble  |  0xD3                                                        |
  +---+-----------+--------------------------------------------------------------+
  | 1 | Reserved  |  000000 (6 bit)                                              |
  +---+-----------+--------------------------------------------------------------+
  | 2 | Length    |  10 bit, # bytes in data message                             |
  +---+-----------+--------------------------------------------------------------+
  | 3 | Data      |  Data message as defined below                               |
  +---+-----------+--------------------------------------------------------------+
  | 4 | CRC       |  3 byte                                                      |
  +---+-----------+--------------------------------------------------------------+


3.2. IMU
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  +---+-------------+----------+------------------+----------------------------------------------------+
  |   | Field       |  Type    |  Units           |  Description                                       |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 0 | Message #   |  uint12  |  4058            |  ANELLO Photonics custom message number            |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 1 | Sub Type ID |  uint4   |  1               |                                                    |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 2 | MCU Time    |  uint64  |  ns              |  Time since power on                               |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 3 | ODR Time    |  int64   |  ns              |  Timestamp of ODR reading                          |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 4 | AX          |  int32   |  1/143165577 g   |  X-Axis Acceleration (intended 15g/2^31)           |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 5 | AY          |  int32   |  1/143165577 g   |  Z-Axis Acceleration                               |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 6 | AZ          |  int32   |  1/143165577 g   |  Z-Axis Acceleration                               |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 7 | WX          |  int32   |  1/4772186 deg/s |  X-Axis Angular Rate (MEMS) (intended 450/2^31)    |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 8 | WY          |  int32   |  1/4772186 deg/s |  Y-Axis Angular Rate (MEMS)                        |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 9 | WZ          |  int32   |  1/4772186 deg/s |  Z-Axis Angular Rate (MEMS)                        |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 10| OG_WZ       |  int32   |  1/4772186 deg/s |  High precision optical gyro z-axis angular rate   |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 11| ODR         |  int16   |  0.01 m/s        |  Scaled composite odometer value                   |
  +---+-------------+----------+------------------+----------------------------------------------------+
  | 12| Temp C      |  int16   |  0.01 °C         |  Temperature                                       |
  +---+-------------+----------+------------------+----------------------------------------------------+


3.3. GPS PVT
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The EVK includes two GNSS receivers. This message can be requested from either or both receivers. 
The Antenna ID field indicates which receiver produced the position information. 

  +---+---------------+----------+------------+----------------------------------------------------------+
  |   | Field         |  Type    |  Units     |  Description                                             |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 0 | Message #     |  uint12  |  4058      |                                                          |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 1 | Sub Type ID   |  uint4   |  2         |                                                          |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 2 | Time          |  uint64  |  ns        |  Time since power on                                     |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 3 | GPS Time      |  uint64  |  ns        |  GPS time                                                |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 4 | Latitude      |  int32   |  1e-7 deg  |  Latitude, '+': north, '-': south                        |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 5 | Longitude     |  int32   |  1e-7 deg  |  Longitude, '+': east, '-': west                         |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 6 | Alt ellipsoid |  int32   |  0.001 m   |  Height above ellipsoid                                  |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 7 | Alt msl       |  int32   |  0.001 m   |  Height above mean sea level                             |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 8 | Speed         |  int32   |  0.001 m/s |  Speed                                                   |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 9 | Heading       |  int32   |  0.001 deg |  GNSS Heading (ground track)                             |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 10| Hacc          |  uint32  |  0.001 m   |  Horizontal accuracy                                     |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 11| Vacc          |  uint32  |  0.001 m   |  Vertical accuracy                                       |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 12| Speed acc     |  uint32  |  0.001 m/s |  Speed accuracy                                          |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 13| Hdg acc       |  uint32  |  1e-5 deg  |  Heading accuracy                                        |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 14| PDOP          |  uint16  |  0.01      |  Position dilution of precision                          |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 15| FixType       |  uint8   |            |  0: No Fix, 2: 2D Fix, 3: 3D Fix, 5: Time Only           |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 16| SatNum        |  uint8   |            |  Number of Satellites used in solution                   |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 17| RTK Status    |  uint8   |            |  0: Single Point Positioning, 1: RTK Float, 2: RTK Fixed |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 18| Antenna ID    |  uint8   |            |  Primary or secondary antenna                            |
  +---+---------------+----------+------------+----------------------------------------------------------+


3.4. INS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  |   | Field         |  Type    |  Units     |  Description                                                                                                |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 0 | Message #     |  uint12  |  4058      |                                                                                                             |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 1 | Sub Type ID   |  uint4   |  4         |                                                                                                             |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 2 | Time          |  uint64  |  ns        |  Time since power on                                                                                        |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 3 | GPS Time      |  uint64  |  ns        |  GPS time                                                                                                   |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 4 | Latitude      |  int32   |  1e-7 deg  |  Latitude, '+': north, '-': south                                                                           |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 5 | Longitude     |  int32   |  1e-7 deg  |  Longitude, '+': east, '-': west                                                                            |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 6 | Alt ellipsoid |  int32   |  0.001 m   |  Height above ellipsoid                                                                                     |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 7 | VN            |  int32   |  0.001 m/s |  North Velocity in NED Frame                                                                                |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 8 | VE            |  int32   |  0.001 m/s |  East Velocity in NED Frame                                                                                 |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 9 | VD            |  int32   |  0.001 m/s |  Down Velocity in NED Frame                                                                                 |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 10| Roll          |  int32   |  1e-5 deg  |  Roll Angle, rotation about body frame X                                                                    |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 11| Pitch         |  int32   |  1e-5 deg  |  Pitch Angle, rotation about body frame Y                                                                   |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 12| Heading/yaw   |  int32   |  1e-5 deg  |  Heading Angle, rotation about body frame Z                                                                 |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 13| ZUPT          |  uint8   |            |  0: Moving, 1: Stationary                                                                                   |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  | 14| Status        |  uint8   |            |  0: Attitude Only, 1: Position and Attitude, 2: Position, Attitude, and Heading, 3: RTK Float, 4: RTK Fixed |
  +---+---------------+----------+------------+-------------------------------------------------------------------------------------------------------------+
  

4.  EVK Input Messages
-----------------------------

4.1. APODO Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The configuration port accepts an odometer aiding message which can convey a direction and a speed. Direction may come from transmission position (reverse, drive) 
or from a signed value of the speed. A negative value indicates reverse (motion in the direction 180 degrees from the vehicle heading); a positive value indicates 
forward (motion in the direction of the vehicle heading). The direction field is optional - if no direction is indicated, the direction is assumed to be forward.   

Direction can also be input without a speed. This can be useful when there is no odometer input available, but transmission position is available. This allows the system to 
distinguish between reverse movement and rotating the vehicle 180 degrees before moving. 

When an #APODO is received with a reverse direction indication, the unit will assume the vehicle is in reverse until a packet is received with a forward direction. 
The odometer input unit is user configurable to m/s, mile/h, km/h, f/s. 

**#APODO,<speed>*checksum**

  +---+------------+-----------+--------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                 |
  +---+------------+-----------+--------------------------------------------------------------+
  | 0 | APODO      |           |  Sentence identifier                                         |
  +---+------------+-----------+--------------------------------------------------------------+
  | 1 | <dir>      |           |  '-': reverse, '+': forward                                  |
  +---+------------+-----------+--------------------------------------------------------------+
  | 2 | <speed>    |  <config> |  Speed is a floating point value expressed in ASCII          |
  +---+------------+-----------+--------------------------------------------------------------+

Examples: 
#APODO, -,24*CS 
#APODO, -24*CS 
#APODO, -,-24*CS 
These would all be interpreted as moving in reverse with a speed of 24. 


4.2.  APCFG Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

For more details on configuration parameters and values, see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_.


4.3.  RTCM Data Input 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Standard RTCM messages are forwarded to the ANELLO EVK to enable the GNSS receivers to reach RTK precision. 
The EVK receives standard RTCM3.3 in MSM format, including MSM4, MSM5, and MSM7 messages. The 
ANELLO Python Program provides an NTRIP client which can connect to a standard NTRIP network and forward the
received RTCM messages into the EVK.