Communication & Messaging
===========================

1.  Port Definitions
---------------------

The Anello EVK uses two logical ports for communication. The primary output port is known as the "Data" port, 
where the major output messages are transmitted at a configurable fixed output rate. This
port also serves as the input port for the RTCM correction stream. The "Configuration" port is used 
for configuration messaging, and also serves as the input port for odometer aiding messages.
Communication occurs at a fixed baudrate of 921600 bits per second.

    +--------------------+----------------------------+---------------------------------------+
    | **Logical Port**   |  **Physical Port**         |  **Functions**                        |
    +--------------------+----------------------------+---------------------------------------+
    | Data Port          | Lowest port #, e.g. COM7   | Output Data Messages, Input RTCM Data |
    +--------------------+----------------------------+---------------------------------------+
    | Configuration Port | Highest port #, e.g. COM10 | Odometer Aiding, Configuration        |
    +--------------------+----------------------------+---------------------------------------+
     

2.  Data Output Messages
-------------------------

The Anello EVK communicates with ASCII messages with the following structure:

-	The lead code identifier for each record is '#'.
-	Each log or command is of variable length depending on amount of data and formats.
-	All data fields are delimited by a comma ',' with two exceptions:
-	Each log ends with a hexadecimal checksum preceded by an asterisk and followed by a line termination using the carriage return and line feed characters.  
-	The checksum is simple, just an XOR of all the bytes between # and *, written in hexadecimal.


The following sentences are the primary output sentences.  The APIMU and APINS messages are transmitted at the output data rate (odr) setting. The GNSS
messages are transmitted at 1Hz.

2.1. APIMU Raw IMU Message

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APIMU      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time since power on                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | AX         |  g        |  X-Axis Acceleration                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 3 | AY         |  g        |  Y-Axis Acceleration                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 4 | AZ         |  g        |  Z-Axis Acceleration                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 5 | WX         |  deg/s    |  X-Axis Angular Rate                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 6 | WY         |  deg/s    |  Y-Axis Angular Rate                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 7 | WZ         |  deg/s    |  Z-Axis Angular Rate (MEMS)                                           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 8 | OG_WZ      |  deg/s    |  Z-Axis Angular Rate (Anello Optical Gyro)                            |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 9 | ODR        |  m/s      |  Scaled composite odometer value                                      |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 10| ODR Time   |  ms       |  Timestamp of odometer reading                                        |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 11| Temp C     |  C        |  Temperature                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  
.. note:: firmware before v0.2.1 also has the Optical Gyro voltage in the 8th position between MEMS z rate and Optical Gyro Rate.

2.2. APGPS Raw GNSS Message

  +---+---------------+-----------+-----------------------------------------------------------------------+
  |   | Field         |  Units    |  Description                                                          |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 0 | APGPS         |           |  Sentence identifier                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time          |  ms       |  Time since power on                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 2 | GPS Time      |  ns       |  GPS time in integer nanoseconds                                      |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 3 | Lat           |  deg      |  Degrees of latitude                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 4 | Long          |  deg      |  Degrees of longitude                                                 |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 5 | Alt ellipsoid |  m        |  Height in meters above ellipsoid                                     |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 6 | Alt msl       |  m        |  Height in meters above mean sea level                                |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 7 | Speed         |  m/s      |  Speed in meters per second                                           |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 8 | Heading       |  deg      |  GNSS heading in degrees                                              |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 9 | Hacc          |  m        |  Horizontal accuracy                                                  |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 10| Vacc          |  m        |  Vertical accuracy                                                    |
  +---+---------------+-----------+-----------------------------------------------------------------------+
  | 11| PDOP          |           |                                                                       |
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


2.3. APINS Navigation Solution Message

  +---+----------+--------+--------------------------------------------------------------------------------------+
  |   | Field    |  Units |  Description                                                                         |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 0 | APINS    |        |  Sentence identifier                                                                 |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 1 | Time     |  ms    |  Time since power on                                                                 |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 2 | GPS Time |  ns    |  GPS time in integer nanoseconds                                                     |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 3 | Status   |        |  0: Attitude Only 1: INS (Pos. Only) 2: INS (Full Soln.), 3: RTK Float, 4: RTK Fixed |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 4 | Lat      |  deg   |  Degrees of latitude                                                                 |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 5 | Long     |  deg   |  Degrees of longitude                                                                |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 6 | Height   |  m     |  Height in meters                                                                    |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 7 | VN       |  m/s   |  North velocity in NED frame                                                         |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 8 | VE       |  m/s   |  East velocity in NED frame                                                          |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 9 | VD       |  m/s   |  Down velocity in NED frame                                                          |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 10| Roll     |  deg   |  Roll angle, rotation about body frame X                                             |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 11| Pitch    |  deg   |  Pitch angle, rotation about body frame Y                                            |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 12| Heading  |  deg   |  Heading angle, rotation about body frame Z                                          |
  +---+----------+--------+--------------------------------------------------------------------------------------+
  | 13| ZUPT     |        |  Status of ZUPT Solution                                                             |
  +---+----------+--------+--------------------------------------------------------------------------------------+

.. note::
    GPS Time in nanoseconds is a useful timestamp for sychronizing the Anello EVK with other systems, as
    it is an absolute time.  GPS Time (GPST) is a continuous time scale (no leap seconds) defined by the GPS 
    Control segment on the basis of a set of atomic clocks at the Monitor Stations and onboard the satellites.
    GPS time is synchronised with the UTC at 1 microsecond level (modulo one second), but actually is kept within 25 ns.
    There is currently an 18 second time offset between UTC and GPS time (ahead).

3.  RTCM Data Input 
----------------------

Standard RTCM Messages are forwarded to the Anello EVK to enable the GNSS receivers to reach RTK precision.
The Anello EVK receives standard RTCM3.3 in MSM format, including MSM4, MSM5, and MSM7 messages. The 
Anello Python Program provides an NTRIP client which can connect to a standard NTRIP network and forward the
received RTCM messages into the Anello EVK.

4.  Configuration/Odometer Input
------------------------------------------

The configuration port accepts an odometer aiding message according to the following format. 

**#APODO,<speed>*checksum**

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APODO      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | <speed>    |  <config> |  Speed is a floating point value expressed in ASCII                   |
  +---+------------+-----------+-----------------------------------------------------------------------+

The odometer input unit is user configurable with the Python Tool or the Configuration Messages to m/s, mile/h, km/h, f/s

5.  Configuration Messages
---------------------------

The easiest way to configure the Anello EVK is with the Anello Python Program. The EVK saves all changes made
through the Anello Python Program to non-volatile flash memory. This insures that the unit is properly configured when
used in the field.  

The protocol to dyanmically configure the unit is explained below, which allows for both temporary (RAM)
and permanent setting (FLASH) of configuration parameters.

**#APCFG,<r/w/R/W>,<param>,<value1>,..,<valueN>*checksum**

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APCFG      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 |<read/write>|           |  'r', read  RAM, 'w' write RAM, 'R' read FLASH, 'W' write FLASH       |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | <param>    |           |  See list of aparemters in Advanced Configuration                     |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 3 | <value>    |           |  Values are expressed in ASCII                                        |
  +---+------------+-----------+-----------------------------------------------------------------------+


