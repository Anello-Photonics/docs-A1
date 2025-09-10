Communication & Messaging
===========================

1  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO products are listed below:
    1. Serial (RS-422) 
    2. UART (3.3V)

1.1 Serial Communication Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Default Baud Rate:
    - 460800

RS-232 Voltage Levels: 
    - +/- 7V

Data Format:
    - Data Bits: 8
    - Stop Bits: 1 
    - Parity: None


1.2 Time Synchronization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
1.2.1 External Sync Pulse
""""""""""""""""""""""""""
All ANELLO products include the option for time synchronization via an external sync pulse, e.g. from external PPS signal.
To enable external synchronization, the "Sync Pulse Enable" `Unit Configuration <https://docs-a1.readthedocs.io/en/x3/unit_configuration.html>`_ must be enabled.
Enabling the sync configuration requires the unit to be reset or repowered as the interrupt is set up during MCU initialization. 
When "Sync Pulse Enable" is on, the rising edge of the sync pulse is detected by an interrupt and time-tagged in the IMU message "T_Sync" field.

The sync pulse input can be sent up to 100 Hz with a pulse width of at least 5 ms. 
A voltage level of 3.3 V is standard, but voltages from 1.5 to 5 V are also supported.
See `Mechanicals <https://docs-a1.readthedocs.io/en/x3/mechanicals.html#anello-evk>`_ to find the sync input pin for each product.



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



2.1 APIMU Message
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


3.1 IMU Message (X3)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ANELLO binary packets use a 2-byte preamble followed by a 1-byte message type and a 1-byte length. There is also a 2-byte checksum after the payload.

  +-----------+---------------+---------------------+------------------------------------+-------------+
  | Preamble  | Message Type  |  Length information |  Payload                           |  Checksum   | 
  +-----------+---------------+---------------------+------------------------------------+-------------+
  | 0xC5 0x50 | IMU = 253     |  1-byte             |  Message structure defined below   |  2-byte     | 
  +-----------+---------------+---------------------+------------------------------------+-------------+


The payload for the binary output message is described below

  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  |   | Field       |  Type    |  Units                                         |  Description                                                                |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 0 | MCU Time    |  uint64  |  ns                                            |  Time since power on                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 1 | Sync Time   |  uint64  |  ns                                            |  Timestamp of input sync pulse (if enabled and provided)                    |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 2 | AX1         |  int16   |  g=raw_int16*(accel_range*0.0000305)           |  Scaled sensor accel                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 3 | AY1         |  int16   |  g=raw_int16*(accel_range*0.0000305)           |  Scaled sensor accel                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 4 | AZ1         |  int16   |  g=raw_int16*(accel_range*0.0000305)           |  Scaled sensor accel                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 5 | WX1         |  int16   |  dps=raw_int16*(mems_gyro_range*0.000035)      |  Scaled sensor rate                                                         |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 6 | WY1         |  int16   |  dps=raw_int16*(mems_gyro_range*0.000035)      |  Scaled sensor rate                                                         |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 7 | WZ1         |  int16   |  dps=raw_int16*(mems_gyro_range*0.000035)      |  Scaled sensor rate                                                         |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 8 | OG_WX       |  int32   |  fog_dps = raw_int32 * (mems_gyro_range / 2^31)|  Scaled sensor rate for FOG. MEMS gyro range is last 11 in MEMS Range field |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 9 | OG_WY       |  int32   |  fog_dps = raw_int32 * (mems_gyro_range / 2^31)|  Scaled sensor rate for FOG. MEMS gyro range is last 11 in MEMS Range field |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 10| OG_WZ       |  int32   |  fog_dps = raw_int32 * (mems_gyro_range / 2^31)|  Scaled sensor rate for FOG. MEMS gyro range is last 11 in MEMS Range field |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 11| MAG_X       |  int16   |  mag_G = raw_int16 / 4096.0                    |  Scaled magnetometer data                                                   |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 12| MAG_Y       |  int16   |  mag_G = raw_int16 / 4096.0                    |  Scaled magnetometer data                                                   |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 13| MAG_Z       |  int16   |  mag_G = raw_int16 / 4096.0                    |  Scaled magnetometer data                                                   |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 14| Temperature |  int16   |  °C * 100                                      |  Scaled temperature data                                                    |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 15| MEMS Range  |  uint16  |  g and dps                                     |  First 5 bits accel range, next 11 bits rate range                          |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 16| FOG Range   |  uint16  |  dps                                           |  FOG range in degrees per second                                            |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 17| Status_X    | Bitfield |                                                | Status based on bits:                                                       |
  |   |             |          |                                                | - Bit 0: Gyro discrepency                                                   |
  |   |             |          |                                                | - Bit 1: Temperature uncontrolled                                           |
  |   |             |          |                                                | - Bit 2: Over current error                                                 |
  |   |             |          |                                                | - Bit 3: SiPhOG supply voltage bad                                          |
  |   |             |          |                                                | - Bits 4-7: RESERVED                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 18| Status_Y    | Bitfield |                                                | Status based on bits:                                                       |
  |   |             |          |                                                | - Bit 0: Gyro discrepency                                                   |
  |   |             |          |                                                | - Bit 1: Temperature uncontrolled                                           |
  |   |             |          |                                                | - Bit 2: Over current error                                                 |
  |   |             |          |                                                | - Bit 3: SiPhOG supply voltage bad                                          |
  |   |             |          |                                                | - Bits 4-7: RESERVED                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+
  | 19| Status_Z    | Bitfield |                                                | Status based on bits:                                                       |
  |   |             |          |                                                | - Bit 0: Gyro discrepency                                                   |
  |   |             |          |                                                | - Bit 1: Temperature uncontrolled                                           |
  |   |             |          |                                                | - Bit 2: Over current error                                                 |
  |   |             |          |                                                | - Bit 3: SiPhOG supply voltage bad                                          |
  |   |             |          |                                                | - Bits 4-7: RESERVED                                                        |
  +---+-------------+----------+------------------------------------------------+-----------------------------------------------------------------------------+




4  Input Messages
-----------------------------

4.1 APCFG Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to configure an ANELLO unit is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/x3/python_tool.html#unit-configurations>`__, 
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

For more details on configuration parameters and values, see `Unit Configurations <https://docs-a1.readthedocs.io/en/x3/unit_configuration.html>`_.




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

