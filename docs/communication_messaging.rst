Communication & Messaging
===========================

1  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO products are listed below:

    1. ANELLO EVK: Serial (USB C), UDP (Ethernet)
    
    2. ANELLO GNSS INS: Serial (RS-232), UDP (Automotive Ethernet)

    3. ANELLO IMU/IMU+: Serial (RS-232)

    4. ANELLO X3: Serial (RS-422), UART (3.3V)

1.1 Serial Communication Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Default Baud Rate:
    - EVK: 921600
    - GNSS INS and IMU/IMU+: 230400
    - X3: 460800

RS-232 Voltage Levels: 
    - +/- 7V

Data Format:
    - Data Bits: 8
    - Stop Bits: 1 
    - Parity: None

1.2 Port Definitions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For all interfaces, there are two main port types. 

1. The data port is where the main ANELLO output messages are transmitted, and also serves as the input port for the RTCM correction stream.
2. The configuration port is used for inputting configuration messaging, inputting odometer messages, and outputting NMEA messages (firmware v1.2 and later).

For UDP interface, there is a third "odometer" port, which is strictly used to send odometer messages. 
This option can be used to feed in odometer data if using ANELLO Python Program to record data over serial, 
in order to keep the serial configuration port free.

    +--------------------+------------------------------------------+---------------------------------------+
    | **Logical Port**   |  **Physical Port**                       |  **Functions**                        |
    +--------------------+------------------------------------------+---------------------------------------+
    | Data Port          | Lowest serial port #, e.g. COM7          | Output: Data Messages                 |
    |                    +------------------------------------------+                                       |
    |                    | UDP: EVK port 1, computer port selectable| Input: RTCM Data                      |
    +--------------------+------------------------------------------+---------------------------------------+
    | Configuration Port | Highest serial port #, e.g. COM10        | Output: NMEA Messages (if configured) |
    |                    +------------------------------------------+                                       |
    |                    | UDP: EVK port 2, computer port selectable| Input: Odometer Data, Configuration   |
    +--------------------+------------------------------------------+---------------------------------------+
    | Odometer Port      | UDP: EVK port 3, computer port selectable| Odometer Channel (UDP Only)           |
    +--------------------+------------------------------------------+---------------------------------------+

UDP communication uses fixed port numbers on the EVK but selectable ports on the external device.
These ports, along with IP addresses and other UDP settings should be configured (see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_).

 .. note:: 
  The "lowest" and "highest" serial ports mentioned above refer to the EVK, which uses an FTDI chip to create 4 virtual COM ports.
  The GNSS INS and IMU/IMU+ have two RS-232 connections, where RS232-1 is the data port and RS232-2 is the configuration port. 
  The port numbers that appear once connected to the computer are determined by how the OS assigns the ports, and therefore the 
  Data port is not necessarily the lowest port # like in the EVK.

1.3 Time Synchronization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1.3.1 External Sync Pulse
""""""""""""""""""""""""""
All ANELLO products include the option for time synchronization via an external sync pulse, e.g. from external PPS signal.
To enable external synchronization, the "Sync Pulse Enable" `Unit Configuration <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_ must be enabled.
Enabling the sync configuration requires the unit to be reset or repowered as the interrupt is set up during MCU initialization. 
When "Sync Pulse Enable" is on, the rising edge of the sync pulse is detected by an interrupt and time-tagged in the IMU message "T_Sync" field.

The sync pulse input can be sent up to 100 Hz with a pulse width of at least 5 ms. 
A voltage level of 3.3 V is standard, but voltages from 1.5 to 5 V are also supported.
See `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html#anello-evk>`_ to find the sync input pin for each product.

1.3.2 PPS Synchronization
""""""""""""""""""""""""""
For the ANELLO GNSS INS and EVK products which contain an internal GNSS receiver, a PPS output pulse is also supplied for time synchronization.
PPS is output directly from the GNSS receiver, which will continue outputting a PPS pulse even if a GPS time fix is lost. 
Note that the PPS accuracy will degrade with time, with drifts around 1 us per second without GPS.
See `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html#anello-evk>`_ to find the PPS output pin for each product.

The PPS rising edge is also detected by an interrupt in the firmware. At the time of the interrupt, the MCU time is recorded by the firmware.
The MCU time of the PPS interrupt is placed into the APINS message. 


2  ASCII Data Output Messages
---------------------------------

ANELLO has two message formats, ASCII and RTCM. The structures of all ASCII messages use the 
following conventions:

-	The lead code identifier for each record is '#'
-	All data fields are delimited by a comma
- Each log ends with a hexadecimal checksum preceded by an asterisk and followed by a line termination using the carriage return and line feed characters
- The checksum is an XOR of all the bytes between # and \*, written in hexadecimal (letters must be uppercase)
- APIMU messages are transmitted at the output data rate (ODR) setting
- APINS messages are transmitted at 100 Hz
- APGPS messages are transmitted at 4 Hz


2.1.1 APIMU Message (EVK & GNSS INS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APIMU message is the IMU output message for EVK and GNSS INS units only.

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APIMU      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | T_Sync     |  ms       |  Time at last sync rising edge (zero when sync config disabled)       |
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
  | 11| ODO Time   |  ms       |  Timestamp of Odometer Reading                                        |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 12| Temp       |  °C       |  Temperature                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  
.. note:: Firmware before v1.0.39 does not have T_Sync field.

2.1.2 APIMU Message (X3)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This is the same as APIMU but with additional Optical Gyro Rates for 3 axes, magnetic field measurements, and without odometer values. This is the output message for X3 units only.
  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APIMU      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | T_Sync     |  ms       |  Time at last sync rising edge (zero when sync config disabled)       |
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
  | 9 | OG_WX      |  deg/s    |  High Precicision X-Axis Angular Rate (ANELLO Optical Gyro)           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 10| OG_WY      |  deg/s    |  High Precicision Y-Axis Angular Rate (ANELLO Optical Gyro)           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 11| OG_WZ      |  deg/s    |  High Precicision Z-Axis Angular Rate (ANELLO Optical Gyro)           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 12| MAG_X      |  g        |  X-Axis Magnetic Field Measurement                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 13| MAG_Y      |  g        |  X-Axis Magnetic Field Measurement                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 14| MAG_Z      |  g        |  X-Axis Magnetic Field Measurement                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 15| Temp C     |  °C       |  Temperature                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 16| Status_X   | Bitfield  |  Status based on bits:                                                |
  |   |            |           |  - Bit 0: Gyro discrepency                                            |
  |   |            |           |  - Bit 1: Temperature uncontrolled                                    |
  |   |            |           |  - Bit 2: Over current error                                          |
  |   |            |           |  - Bit 3: SiPhOG supply voltage bad                                   |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 17| Status_Y   | Bitfield  |  Status based on bits:                                                |
  |   |            |           |  - Bit 0: Gyro discrepency                                            |
  |   |            |           |  - Bit 1: Temperature uncontrolled                                    |
  |   |            |           |  - Bit 2: Over current error                                          |
  |   |            |           |  - Bit 3: SiPhOG supply voltage bad                                   |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 18| Status_Z   | Bitfield  |  Status based on bits:                                                |
  |   |            |           |  - Bit 0: Gyro discrepency                                            |
  |   |            |           |  - Bit 1: Temperature uncontrolled                                    |
  |   |            |           |  - Bit 2: Over current error                                          |
  |   |            |           |  - Bit 3: SiPhOG supply voltage bad                                   |
  +---+------------+-----------+-----------------------------------------------------------------------+



2.1.3 APIM1 Message (IMU & IMU+)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APIM1 message is the same as APIMU but without odometer values. This is the output message for IMU and IMU+ units only.

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APIMU      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | T_Sync     |  ms       |  Time at last sync rising edge (zero when sync config disabled)       |
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
  | 12| Temp C     |  °C       |  Temperature                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  
.. note:: Firmware before v1.0.39 does not have T_Sync field.

2.2 APGPS Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APGPS message is the PVT output from the EVK and GNSS INS units only.

  +---+---------------+-----------+-----------------------------------------------------------------------+
  |   | Field         |  Units    |  Description                                                          |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 0 | APGPS         |           |  Sentence identifier                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time          |  ms       |  Time since power on                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 2 | GPS Time      |  ns       |  GPS Time in integer ns                                               |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 3 | Lat           |  deg      |  Latitude, '+': north, '-': south                                     |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 4 | Long          |  deg      |  Longitude, '+': east, '-': west                                      |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 5 | Alt ellipsoid |  m        |  Height above ellipsoid                                               |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 6 | Alt msl       |  m        |  Height above mean sea level                                          |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 7 | Speed         |  m/s      |  GNSS Speed                                                           |
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
  | 14| Speed Acc     |           |  Accuracy of GNSS Speed measurement                                   |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 15| Hdg Acc       |           |  Accuracy of GNSS Heading measurement                                 |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 16| RTK Status    |           |  0: Single Point Positioning, 1: RTK Float, 2: RTK Fixed              |
  +---+---------------+-----------+-----------------------------------------------------------------------+

.. note:: This packet should be used to correlate GPS time and system time. The packet is time stamped at the time the PPS signal is generated by the GNSS receiver.


2.3 APHDG Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APHDG message contains dual heading information from the dual GNSS receivers if both ANT1 and ANT2 are connected. 
This message is output from the EVK and GNSS INS units only.

  +---+------------------------+-----------+-----------------------------------------------------------------------+
  |   | Field                  |  Units    |  Description                                                          |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 0 | APHDG                  |           |  Sentence identifier                                                  |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time                   |  ms       |  Time since power on                                                  |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 2 | GPS Time               |  ns       |  GPS Time in integer ns (not UTC time)                                |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 3 | relPosN                |  m        |  North component of relative position vector                          |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 4 | relPosE                |  m        |  East component of relative position vector                           |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 5 | relPosD                |  m        |  Down component of relative position vector                           |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 6 | relPosLength           |  m        |  Length of relative position vector between antennae                  |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 7 | relPosHeading          |  deg      |  Heading from primary antenna to secondary antenna                    |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 8 | RelPosLength Accuracy  |  m        |  Accuracy of dual antennae baseline length                            |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 9 | relPosHeading Accuracy |  deg      |  Accuracy of dual antennae heading                                    |
  +---+------------------------+-----------+-----------------------------------------------------------------------+
  | 10| flags                  |           |  Status based on bits:                                                |
  |   |                        |           |  - Bit 0: gnssFixOK                                                   |
  |   |                        |           |  - Bit 1: diffSoln                                                    |
  |   |                        |           |  - Bit 2: relPosValid                                                 |
  |   |                        |           |  - Bits 4..3: carrSoln                                                |
  |   |                        |           |  - Bit 5: isMoving                                                    |
  |   |                        |           |  - Bit 6: refPosMiss                                                  |
  |   |                        |           |  - Bit 7: refObsMiss                                                  |
  |   |                        |           |  - Bit 8: relPosHeading Valid                                         |
  |   |                        |           |  - Bit 9: relPos Normalized                                           |
  +---+------------------------+-----------+-----------------------------------------------------------------------+


2.4 APINS Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APINS message is the Kalman filter position, velocity, and attitude solution output from the EVK and GNSS INS units.

  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                                                                                   |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 0 | APINS      |           |  Sentence identifier                                                                                                           |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                                                                           |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 2 | PPS Time   |  ns       |  Time of last PPS pulse converted to GPS time (time since midnight on Jan 6, 1980)                                             |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 3 | Status     |           |  0: Attitude Only; 1: Position and Attitude; 2: Position, Attitude, and Heading; 3: RTK Float; 4: RTK Fix                      |
  |   |            |           |  If GPS button is turned OFF in Python tool, 8: Attitude Only; 9: Position and Attitude; 10: Position, Attitude, and Heading   |  
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 4 | Lat        |  deg      |  Latitude, '+': North, '-': South                                                                                              |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 5 | Long       |  deg      |  Longitude, '+': East, '-': West                                                                                               |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 6 | Height     |  m        |  Height above ellipsoid                                                                                                        |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 7 | VN         |  m/s      |  North Velocity in NED Frame                                                                                                   |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 8 | VE         |  m/s      |  East Velocity in NED Frame                                                                                                    |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 9 | VD         |  m/s      |  Down Velocity in NED Frame                                                                                                    |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 10| Roll       |  deg      |  Roll Angle, rotation about body frame X                                                                                       |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 11| Pitch      |  deg      |  Pitch Angle, rotation about body frame Y                                                                                      |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 12| Heading    |  deg      |  Heading Angle, rotation about body frame Z                                                                                    |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 13| ZUPT       |           |  0: Moving, 1: Stationary                                                                                                      |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+

.. note:: Roll, pitch and heading angles are calculated as standard aerospace Euler angles in a 3-2-1 (yaw, pitch, roll) body frame rotation.


3  RTCM Binary Data Output Messages
--------------------------------------

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

The ANELLO Python Tool handles logging and decoding of the RTCM binary format. 
However, an `RTCM decoder <https://github.com/Anello-Photonics/decoder/blob/master/decoder.cpp>`_ is provided if needed,
with the checksum definition found `here <https://github.com/Anello-Photonics/decoder/blob/master/artcm/artcm.c>`_.


3.1.1 IMU Message (EVK & GNSS INS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The IMU output message for EVK and GNSS INS units has a subtype ID of 1.

  +---+-------------+----------+------------------+----------------------------------------------------------+
  |   | Field       |  Type    |  Units           |  Description                                             |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 0 | Message #   |  uint12  |  4058            |  ANELLO Photonics custom message number                  |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 1 | Sub Type ID |  uint4   |  1               |                                                          |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 2 | MCU Time    |  uint64  |  ns              |  Time since power on                                     |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 3 | Sync Time   |  uint64  |  ns              |  Timestamp of input sync pulse (if enabled and provided) |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 4 | ODO Time    |  uint64  |  ns              |  Timestamp of odometer reading                           |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 5 | AX          |  int32   |  1/143165577 g   |  X-Axis Acceleration (intended 15g/2^31)                 |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 6 | AY          |  int32   |  1/143165577 g   |  Y-Axis Acceleration                                     |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 7 | AZ          |  int32   |  1/143165577 g   |  Z-Axis Acceleration                                     |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 8 | WX          |  int32   |  1/4772186 deg/s |  X-Axis Angular Rate (MEMS) (intended 450/2^31)          |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 9 | WY          |  int32   |  1/4772186 deg/s |  Y-Axis Angular Rate (MEMS)                              |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 10| WZ          |  int32   |  1/4772186 deg/s |  Z-Axis Angular Rate (MEMS)                              |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 11| OG_WZ       |  int32   |  1/4772186 deg/s |  High precision optical gyro z-axis angular rate         |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 12| ODO         |  int16   |  0.01 m/s        |  Scaled composite odometer value                         |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 13| Temp C      |  int16   |  0.01 °C         |  Temperature                                             |
  +---+-------------+----------+------------------+----------------------------------------------------------+

3.1.2 IMU Message (X3)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ANELLO binary packets use a 2-byte preamble followed by a 1-byte message type and a 1-byte length. There is also a 2-byte checksum after the payload.

  +-----------+---------------+---------------------+------------------------------------+-------------+
  | Preamble  | Message Type  |  Length information |  Payload                           |  Checksum   | 
  +-----------+---------------+---------------------+------------------------------------+-------------+
  | 0xC5 0x50 | IMU = 253     |  1-byte             |  Message structure defined below   |  2-byte     | 
  +-----------+---------------+---------------------+------------------------------------+-------------+


The payload for the binary output message is described below

  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  |   | Field       |  Type    |  Units                     |  Description                                             |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 0 | MCU Time    |  uint64  |  ns                        |  Time since power on                                     |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 1 | Sync Time   |  uint64  |  ns                        |  Timestamp of input sync pulse (if enabled and provided) |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 2 | AX1         |  int16   |  g=value*(range*0.0000305) |  Scaled sensor accel                                     |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 3 | AY1         |  int16   |  g=value*(range*0.0000305) |  Scaled sensor accel                                     |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 4 | AZ1         |  int16   |  g=value*(range*0.0000305) |  Scaled sensor accel                                     |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 5 | WX1         |  int16   |  dps=value*(range*0.000035)|  Scaled sensor rate                                      |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 6 | WY1         |  int16   |  dps=value*(range*0.000035)|  Scaled sensor rate                                      |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 7 | WZ1         |  int16   |  dps=value*(range*0.000035)|  Scaled sensor rate                                      |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 8 | OG_WX       |  int32   |  dps * 10000000            |  Scaled sensor rate for FOG                              |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 9 | OG_WY       |  int32   |  dps * 10000000            |  Scaled sensor rate for FOG                              |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 10| OG_WZ       |  int32   |  dps * 10000000            |  Scaled sensor rate for FOG                              |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 11| MAG_X       |  int16   |  g * 4096                  |  Scaled magnetometer data                                |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 12| MAG_Y       |  int16   |  g * 4096                  |  Scaled magnetometer data                                |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 13| MAG_Z       |  int16   |  g * 4096                  |  Scaled magnetometer data                                |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 14| Temperature |  int16   |  °C * 100                  |  Scaled temperature data                                 |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 15| MEMS Range  |  uint16  |  g and dps                 |  First 5 bits accel range, next 11 bits rate range       |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 16| FOG Range   |  uint16  |  dps                       |  FOG range in DPS                                        |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 17| Status_X    | Bitfield |                            | Status based on bits:                                    |
  |   |             |          |                            | - Bit 0: Gyro discrepency                                |
  |   |             |          |                            | - Bit 1: Temperature uncontrolled                        |
  |   |             |          |                            | - Bit 2: Over current error                              |
  |   |             |          |                            | - Bit 3: SiPhOG supply voltage bad                       |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 18| Status_Y    | Bitfield |                            | Status based on bits:                                    |
  |   |             |          |                            | - Bit 0: Gyro discrepency                                |
  |   |             |          |                            | - Bit 1: Temperature uncontrolled                        |
  |   |             |          |                            | - Bit 2: Over current error                              |
  |   |             |          |                            | - Bit 3: SiPhOG supply voltage bad                       |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+
  | 19| Status_Z    | Bitfield |                            | Status based on bits:                                    |
  |   |             |          |                            | - Bit 0: Gyro discrepency                                |
  |   |             |          |                            | - Bit 1: Temperature uncontrolled                        |
  |   |             |          |                            | - Bit 2: Over current error                              |
  |   |             |          |                            | - Bit 3: SiPhOG supply voltage bad                       |
  +---+-------------+----------+----------------------------+----------------------------------------------------------+


3.1.3 IMU Message (IMU & IMU+)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The IMU output message for IMU and IMU+ units has a subtype ID of 6. 
It is the same as IMU message for the EVK and GNSS INS but without odometer values.

  +---+-------------+----------+------------------+----------------------------------------------------------+
  |   | Field       |  Type    |  Units           |  Description                                             |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 0 | Message #   |  uint12  |  4058            |  ANELLO Photonics custom message number                  |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 1 | Sub Type ID |  uint4   |  6               |                                                          |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 2 | MCU Time    |  uint64  |  ns              |  Time since power on                                     |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 3 | Sync Time   |  uint64  |  ns              |  Timestamp of input sync pulse (if enabled and provided) |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 4 | AX          |  int32   |  1/143165577 g   |  X-Axis Acceleration (intended 15g/2^31)                 |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 5 | AY          |  int32   |  1/143165577 g   |  Y-Axis Acceleration                                     |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 6 | AZ          |  int32   |  1/143165577 g   |  Z-Axis Acceleration                                     |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 7 | WX          |  int32   |  1/4772186 deg/s |  X-Axis Angular Rate (MEMS) (intended 450/2^31)          |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 8 | WY          |  int32   |  1/4772186 deg/s |  Y-Axis Angular Rate (MEMS)                              |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 9 | WZ          |  int32   |  1/4772186 deg/s |  Z-Axis Angular Rate (MEMS)                              |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 10| OG_WZ       |  int32   |  1/4772186 deg/s |  High precision optical gyro z-axis angular rate         |
  +---+-------------+----------+------------------+----------------------------------------------------------+
  | 11| Temp C      |  int16   |  0.01 °C         |  Temperature                                             |
  +---+-------------+----------+------------------+----------------------------------------------------------+


3.3 GPS PVT Message (EVK/GNSS INS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The GPS message is the PVT output from the EVK and GNSS INS units only. 
The Antenna ID field indicates which receiver (that connected to ANT1 or ANT2) produced the position information. 

  +---+---------------+----------+------------+----------------------------------------------------------+
  |   | Field         |  Type    |  Units     |  Description                                             |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 0 | Message #     |  uint12  |  4058      |                                                          |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 1 | Sub Type ID   |  uint4   |  2         |                                                          |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 2 | Time          |  uint64  |  ns        |  Time since power on                                     |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 3 | GPS Time      |  uint64  |  ns        |  GPS time (GTOW) – Time since Jan 6, 1980                |
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
  | 12| Hdg acc       |  uint32  |  1e-5 deg  |  Heading accuracy                                        |
  +---+---------------+----------+------------+----------------------------------------------------------+
  | 13| Speed acc     |  uint32  |  0.001 m/s |  Speed accuracy                                          |
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

3.4 HDG Message (EVK/GNSS INS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The HDG message contains dual heading information from the dual GNSS receivers if both ANT1 and ANT2 are connected. 
This message is output from the EVK and GNSS INS units only.

  +---+------------------------+----------+------------------+----------------------------------------------------------+
  |   | Field                  |  Type    |  Units           |  Description                                             |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 0 | Message #              |  uint12  |  4058            |  ANELLO Photonics custom message number                  |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 1 | Sub Type ID            |  uint4   |  3               |                                                          |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 2 | MCU Time               |  uint64  |  ns              |  Time since power on                                     |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 3 | GPS Time               |  uint64  |  ns              |  GPS time (GTOW) – Time since Jan 6, 1980                |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 4 | relPosN                |  int32   |  0.01 m          |  North component of relative position vector             |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 5 | relPosE                |  int32   |  0.01 m          |  East component of relative position vector              |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 6 | relPosD                |  int32   |  0.01 m          |  Down component of relative position vector              |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 7 | relPosLength           |  int32   |  0.01 m          |  Length of relative position vector between antennae     |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 8 | relPosHeading          |  int32   |  1e-5 deg        |  Heading from primary antenna to secondary antenna       |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 9 | relPosLength Accuracy  |  uint32  |  0.1 mm          |  Accuracy of dual antennae baseline length               |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 10| relPosHeading Accuracy |  uint32  |  1e-5 deg        |  Accuracy of dual antennae heading                       |
  +---+------------------------+----------+------------------+----------------------------------------------------------+
  | 11| flags                  |  uint16_t|                  |  Status based on bits:                                   |
  |   |                        |          |                  |  - Bit 0: gnssFixOK                                      |
  |   |                        |          |                  |  - Bit 1: diffSoln                                       |
  |   |                        |          |                  |  - Bit 2: relPosValid                                    |
  |   |                        |          |                  |  - Bits 4..3: carrSoln                                   |
  |   |                        |          |                  |  - Bit 5: isMoving                                       |
  |   |                        |          |                  |  - Bit 6: refPosMiss                                     |
  |   |                        |          |                  |  - Bit 7: refObsMiss                                     |
  |   |                        |          |                  |  - Bit 8: relPosHeading Valid                            |
  |   |                        |          |                  |  - Bit 9: relPos Normalized                              |
  +---+------------------------+----------+------------------+----------------------------------------------------------+


3.5 INS Message (EVK/GNSS INS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The INS message is the Kalman filter position, velocity, and attitude solution output from the EVK and GNSS INS units.

  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  |   | Field         |  Type    |  Units     |  Description                                                                                                                 |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 0 | Message #     |  uint12  |  4058      |                                                                                                                              |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 1 | Sub Type ID   |  uint4   |  4         |                                                                                                                              |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 2 | Time          |  uint64  |  ns        |  Time since power on                                                                                                         |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 3 | PPS Time      |  uint64  |  ns        |  Time of last PPS pulse converted to GPS time (time since midnight on Jan 6, 1980)                                           |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 4 | Latitude      |  int32   |  1e-7 deg  |  Latitude, '+': north, '-': south                                                                                            |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 5 | Longitude     |  int32   |  1e-7 deg  |  Longitude, '+': east, '-': west                                                                                             |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 6 | Alt ellipsoid |  int32   |  0.001 m   |  Height above ellipsoid                                                                                                      |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 7 | VN            |  int32   |  0.001 m/s |  North Velocity in NED Frame                                                                                                 |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 8 | VE            |  int32   |  0.001 m/s |  East Velocity in NED Frame                                                                                                  |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 9 | VD            |  int32   |  0.001 m/s |  Down Velocity in NED Frame                                                                                                  |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 10| Roll          |  int32   |  1e-5 deg  |  Roll Angle, rotation about body frame X                                                                                     |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 11| Pitch         |  int32   |  1e-5 deg  |  Pitch Angle, rotation about body frame Y                                                                                    |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 12| Heading       |  int32   |  1e-5 deg  |  Heading Angle, rotation about body frame Z                                                                                  |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 13| ZUPT          |  uint8   |            |  0: Moving, 1: Stationary                                                                                                    |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 14| Status        |  uint8   |            |  0: Attitude Only; 1: Position and Attitude; 2: Position, Attitude, and Heading; 3: RTK Float; 4: RTK Fix                    |
  |   |               |          |            |  If GPS button is turned OFF in Python tool, 8: Attitude Only; 9: Position and Attitude; 10: Position, Attitude, and Heading |  
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  

4  Input Messages
-----------------------------

4.1 APCFG Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to configure an ANELLO unit is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/latest/python_tool.html#unit-configurations>`__, 
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

For more details on configuration parameters and values, see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_.

4.2 APVEH Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to set ANELLO vehicle configurations is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/latest/python_tool.html#vehicle-configurations>`__, 
which saves all changes to non-volatile flash memory. 

Alternatively, the unit can be configured using the APVEH message, which allows for both temporary (RAM) and permanent setting (FLASH) of configuration parameters.

**#APVEH,<r/w/R/W>,<param1>,<value1>,...,<paramN>,<valueN>*checksum**

  +---+------------+-------------------------------------------------------------------------------------+
  |   | Field      |  Description                                                                        |
  +---+------------+-------------------------------------------------------------------------------------+
  | 0 | APVEH      |  Sentence identifier                                                                |
  +---+------------+-------------------------------------------------------------------------------------+
  | 1 |<read/write>|  'r': read  RAM, 'w': write RAM, 'R': read FLASH, 'W': write FLASH                  |
  +---+------------+-------------------------------------------------------------------------------------+
  | 2 | <param>    |  Configuration parameter (APVEH code)                                               |
  +---+------------+-------------------------------------------------------------------------------------+
  | 3 | <value>    |  Configuration value, expressed in ASCII                                            |
  +---+------------+-------------------------------------------------------------------------------------+
  | 4 | checksum   |  XOR of bytes between # and \* written in hexadecimal (letters must be uppercase)   |
  +---+------------+-------------------------------------------------------------------------------------+

4.3 APODO Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The ANELLO EVK and GNSS INS accepts odometer input over the configuration port.
The APODO message is in ASCII format and used to convey the vehicle direction and a speed.
A negative value indicates reverse, and a positive value indicates forward. If no direction is indicated, the direction is assumed to be forward.   

Direction can also be input without a speed. This can be useful when there is no odometer input available, but transmission position is available. 
This is useful to enable INS initialization in both forward and reverse. 

When an APODO message is received with a reverse direction indication, the unit will assume the vehicle is in reverse until a packet is received with a forward direction. 
The units of the speed in the APODO message is user configurable to m/s (default), mile/hr, km/hr, ft/s 
(see 'odo' code in `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_).

**#APODO,<dir>,<speed>*checksum**

  +---+------------+------------------------------------------------------------------------------------+
  |   | Field      |  Description                                                                       |
  +---+------------+------------------------------------------------------------------------------------+
  | 0 | APODO      |  Sentence identifier                                                               |
  +---+------------+------------------------------------------------------------------------------------+
  | 1 | <dir>      |  '-': reverse, '+': forward (optional)                                             |
  +---+------------+------------------------------------------------------------------------------------+
  | 2 | <speed>    |  Speed is a floating point value, units are set in unit configurations             |
  +---+------------+------------------------------------------------------------------------------------+
  | 3 | checksum   |  XOR of bytes between # and \* written in hexadecimal (letters must be uppercase)  |
  +---+------------+------------------------------------------------------------------------------------+

For example, the following would all be interpreted as moving in reverse with a speed of 24: 
#APODO,-,24*7E 
#APODO,-24*52
#APODO,-,-24*53


.. note:: If sending odometer speeds by UDP from another program, send to UDP port 3 on the EVK, from the computer's UDP port matching "odometer port" configuration.


4.4 RTCM Data Input 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Standard RTCM messages can be forwarded to the data port of the ANELLO EVK and GNSS INS to enable the GNSS receivers to reach RTK precision. 
Standard RTCM3.3 in MSM format, including MSM4, MSM5, and MSM7 messages, are supported. 
The ANELLO Python Program provides an NTRIP client which can connect to a standard NTRIP network and forward the RTCM messages to the ANELLO unit.


4.5 Ping Command
~~~~~~~~~~~~~~~~~~~~~~~~~~
The Ping command can be used to test if the serial port is properly configured.

#APPNG*48

A correctly received ping command generates a response from the unit of: 

#APPNG,0*54


4.6 Echo Command
~~~~~~~~~~~~~~~~~~~~~~~~~~
The Echo command serves as an additional communication test for the serial port configuration as well as the checksum generator. For example:

#APECH,Echo! echo… ech… e…\*77

A correctly received Echo command generates an identical response from the unit: 

#APECH,Echo! echo… ech… e…\*77.


4.7 Reset Command
~~~~~~~~~~~~~~~~~~~~~~~~~~
The reset command allows the user to reset the system, e.g. after changing a configuration setting that requires a power cycle. 
No response message is generated; however, the system will reset causing the system output to be suspended briefly. 

#APRST,0*58 


5  Error Messages
-----------------------------

If an incorrect command is sent to the unit, it responds with one of ten error responses. The error message format is: 

#APERR,<error code>*CS 

The following table lists the error code along with the corresponding description. 

+------------+--------------------------------------------+
| Error Code | Description                                |
+============+============================================+
| 1          | No start character (#)                     |
+------------+--------------------------------------------+
| 2          | Read/Write indicator missing (from #APCFG  |
|            | or #APVEH)                                 |
+------------+--------------------------------------------+
| 3          | Incomplete message (checksum missing)      |
+------------+--------------------------------------------+
| 4          | Incorrect checksum                         |
+------------+--------------------------------------------+
| 5          | Invalid preamble (AP)                      |
+------------+--------------------------------------------+
| 6          | Invalid message type                       |
+------------+--------------------------------------------+
| 7          | Invalid field                              |
+------------+--------------------------------------------+
| 8          | Invalid value                              |
+------------+--------------------------------------------+
| 9          | Flash locked                               |
+------------+--------------------------------------------+
| 10         | Unexpected character (applies to APPID,    |
|            | APSTA, APVER, APSER, APFSN, and APFHW)     |
+------------+--------------------------------------------+
| 11         | Disabled command (applies to APODO)        |
+------------+--------------------------------------------+

6  Checksum
----------------------------

6.1 Ascii Checksum
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The ASCII checksum is an XOR of all characters between the start character ‘#’ and the checksum indicator ‘*’. The following python code snippet can be used to generate the correct checksum.

.. code-block:: python

    # Specify the input message (generates the string: #APCFG,W,odr,2,msg,IMU*4B)
    msg = b’APCFG,W,odr,2,msg,IMU’

    # Generate the checksum for the inertial product input message
    checksum = 0
    for c in msg:
      checksum = checksum ^ int(c)

    # Print the complete message (starting field and checksum) to the screen
    print(‘#%s*%02X’ % (msg.decode(), checksum))


6.2 Binary Checksum
~~~~~~~~~~~~~~~~~~~~~~~~~~~~


GNSS / INS, EVK, IMU / IMU+:
  The checksum definition can befound `here <https://github.com/Anello-Photonics/decoder/blob/master/artcm/artcm.c>`_.


X3:
  The 2 preamble bytes and the checksum itself are not included in the checksum calculation.
  Checksum is calculated as follows, where N is the number of bytes included in the checksum calculation:

  .. code-block:: python

    CK_A = 0
    CK_B = 0
    for (I = 0; I < N; I++)
    {
    CK_A = CK_A + Buffer[I]
    CK_B = CK_B + CK_A
    }

