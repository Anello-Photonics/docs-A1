Communication & Messaging
===========================

1  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO IMU/IMU+ is serial (RS-232)



1.1 Serial Communication Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Default Baud Rate:  230400



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
These ports, along with IP addresses and other UDP settings should be configured (see `Unit Configurations <https://docs-a1.readthedocs.io/en/imu_plus/unit_configuration.html>`_).

 .. note:: 
  The "lowest" and "highest" serial ports mentioned above refer to the EVK, which uses an FTDI chip to create 4 virtual COM ports.
  The GNSS INS and IMU/IMU+ have two RS-232 connections, where RS232-1 is the configuration port and RS232-2 is the data port. 
  The port numbers that appear once connected to the computer are determined by how the OS assigns the ports, and therefore the 
  Data port is not necessarily the lowest port # like in the EVK.

1.3 Time Synchronization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1.3.1 External Sync Pulse
""""""""""""""""""""""""""
All ANELLO products include the option for time synchronization via an external sync pulse, e.g. from external PPS signal.
To enable external synchronization, the "Sync Pulse Enable" `Unit Configuration <https://docs-a1.readthedocs.io/en/imu_plus/unit_configuration.html>`_ must be enabled.
Enabling the sync configuration requires the unit to be reset or repowered as the interrupt is set up during MCU initialization. 
When "Sync Pulse Enable" is on, the rising edge of the sync pulse is detected by an interrupt and time-tagged in the IMU message "T_Sync" field.

The sync pulse input can be sent up to 100 Hz with a pulse width of at least 5 ms. 
A voltage level of 3.3 V is standard, but voltages from 1.5 to 5 V are also supported.
See `Mechanicals <https://docs-a1.readthedocs.io/en/imu_plus/mechanicals.html#anello-evk>`_ to find the sync input pin for each product.

1.3.2 PPS Synchronization
""""""""""""""""""""""""""
For the ANELLO GNSS INS and EVK products which contain an internal GNSS receiver, a PPS output pulse is also supplied for time synchronization.
PPS is output directly from the GNSS receiver, which will continue outputting a PPS pulse even if a GPS time fix is lost. 
Note that the PPS accuracy will degrade with time, with drifts around 1 us per second without GPS.
See `Mechanicals <https://docs-a1.readthedocs.io/en/imu_plus/mechanicals.html#anello-evk>`_ to find the PPS output pin for each product.

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




2.1 APIM1 Message 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APIM1 message is the same as APIMU but without odometer values. 

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
  

2.2 APAHRS Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APAHRS message is only available with the ANELLO AHRS upgrade on the IMU+. It provides the roll, pitch and yaw angles calculated as standard aerospace Euler angles in a 3-2-1 (yaw, pitch, roll) body frame rotation.

  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                                                                                   |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 0 | APINS      |           |  Sentence identifier                                                                                                           |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                                                                           |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 2 | Sync Time  |  ns       |  Time of the last sync pulse.                                                                                                  |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 3 | Roll       |  deg      |  Roll in degrees                                                                                                               |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 4 | Pitch      |  deg      |  Pitch in degrees                                                                                                              |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 5 | Yaw        |  deg      |  Yaw in degrees                                                                                                                |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+
  | 6 | ZUPT Status|           |  1 if ZUPT is enabled, 0 if ZUPT is disabled                                                                                   |
  +---+------------+-----------+--------------------------------------------------------------------------------------------------------------------------------+

.. note:: The yaw is not an absolute heading but an integrated relative heading - unless an absolute heading is provided by the user, after which the optical gyro integrates relative to that absolute heading.

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



3.1 IM1 Message 
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



3.2 AHRS Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The APAHRS message is only available with the ANELLO AHRS upgrade on the IMU+. It provides the roll, pitch and yaw angles calculated as standard aerospace Euler angles in a 3-2-1 (yaw, pitch, roll) body frame rotation.

  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  |   | Field         |  Type    |  Units     |  Description                                                                                                                 |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 0 | Message #     |  uint12  |  4058      |                                                                                                                              |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 1 | Sub Type ID   |  uint4   |  8         |                                                                                                                              |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 2 | Time          |  uint64  |  ns        |  Time since power on                                                                                                         |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 3 | Sync Time     |  uint64  |  ns        |  Time of last sync pulse.                                                                                                    |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 4 | Roll          |  int32   |  1e-5 deg  |  Roll in degrees                                                                                                             |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 5 | Pitch         |  int32   |  1e-5 deg  |  Pitch in degrees                                                                                                            |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 6 | Yaw           |  int32   |  1e-5 deg  |  Yaw in degrees                                                                                                              |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+
  | 7 | ZUPT Status   |  uint8   |  1 or 0    |  1 if ZUPT is enabled, 0 if ZUPT is disabled                                                                                 |
  +---+---------------+----------+------------+------------------------------------------------------------------------------------------------------------------------------+

.. note:: The yaw is not an absolute heading but an integrated relative heading - unless an absolute heading is provided by the user, after which the optical gyro integrates relative to that absolute heading.



4  Input Messages
-----------------------------

4.1 APCFG Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to configure an ANELLO unit is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/imu_plus/python_tool.html#unit-configurations>`__, 
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

For more details on configuration parameters and values, see `Unit Configurations <https://docs-a1.readthedocs.io/en/imu_plus/unit_configuration.html>`_.



4.2 Ping Command
~~~~~~~~~~~~~~~~~~~~~~~~~~
The Ping command can be used to test if the serial port is properly configured.

#APPNG*48

A correctly received ping command generates a response from the unit of: 

#APPNG,0*54


4.3 Echo Command
~~~~~~~~~~~~~~~~~~~~~~~~~~
The Echo command serves as an additional communication test for the serial port configuration as well as the checksum generator. For example:

#APECH,Echo! echo… ech… e…\*77

A correctly received Echo command generates an identical response from the unit: 

#APECH,Echo! echo… ech… e…\*77.


4.4 Reset Command
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

  - Specify the input message (generates the string: #APCFG,W,odr,2,msg,IMU*4B)
  - Generate the checksum for the inertial product input message
  - Print the complete message (starting field and checksum) to the screen

.. code-block:: python
 
    msg = bytearray(APCFG,W,odr,2,msg,IMU)
    checksum = 0
    for c in msg:
      checksum = checksum ^ int(c)
    print(#%s*%02X % (msg.decode(), checksum))


6.2 Binary Checksum
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The checksum definition can be found `here <https://github.com/Anello-Photonics/decoder/blob/master/artcm/artcm.c>`_.

