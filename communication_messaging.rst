Communication and Messaging
===========================

1.  Port Definitions
---------------------

The Anello A-1 uses two logical ports for communication.  The primary output port is known as the "Data" port,
and it the major output messages are transmitted at a configurable fixed output rate on this port.  The "Data"
port also serves as the input port for the RTCM correction stream.  The control and confiruation port, is used
for for user and configuration messaging, as well as it serves as the input port for odometer aiding messages.

    +-------------------------+-----------------------------------+
    | **Logical Port**        |  **Functions**                    |
    +-------------------------+-----------------------------------+
    |  Data Port  (Output)    | Output Data Messages (IMU,GPS,NAV)|
    +-------------------------+-----------------------------------+
    |  Data Port  (Input)     | RTCM Data Stream                  |
    +-------------------------+-----------------------------------+
    |  Configuration  Port    | Odometer Aiding, Configuration    |
    +-------------------------+-----------------------------------+
     

2.  Data Output Messages
-------------------------

The Anello A-1 communicates with ASCII sentences.
ASCII messages are readable by both the user and a computer. The structures of all ASCII messages follow the 
general conventions as noted here:

-	The lead code identifier for each record is '#'.
-	Each log or command is of variable length depending on amount of data and formats.
-	All data fields are delimited by a comma ',' with two exceptions:
-	Each log ends with a hexadecimal checksum preceded by an asterisk and followed by a line termination using the carriage return and line feed characters.  
-	The checksum is simple, just an XOR of all the bytes between # and *, written in hexadecimal.


The following sentennces are the primary output sentences.  The APIMU and APINS messages are transitted at the output data rate (odr) setting. The GNSS 
messages are transmitted at 1Hz.

2.1. APIMU Raw IMU Message

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APIMU      |  n/a      |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time in milli-seconds since power on                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | AX         |  G        |  X-Axis Acceleration in units of G                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 3 | AY         |  G        |  Y-Axis Acceleration in units of G                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 4 | AZ         |  G        |  Z-Axis Acceleration in units of G                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 5 | WX         |  dps      |  X-Axis Angular Rate in units of deg/sec                              |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 6 | WY         |  dps      |  Y-Axis Angular Rate in units of deg/sec                              |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 7 | WZ         |  dps      |  Z-Axis Angular Rate in units of deg/sec (MEMS)                       |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 8 | OG_VZ      |  Volts    |  High Precicision Z-Axis Angular Rate Volts (Anello Optical Gyro)     |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 9 | OG_WZ      |  dps      |  High Precicision Z-Axis Angular Rate Volts (Anello Optical Gyro)     |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 10| ODR        |  m/s      |  Scaled Composite Odometer Value (m/s)                                |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 11| ODR Time   |  ms       |  Timestampe of Odometer Reading in ms                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+
  

2.2. APGPS Raw GNSS Message

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APGPS      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time in milli-seconds since power on                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | WeekSec    |  ms       |  GPS Week-second time of current GPS weekin milli-seconds             |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 3 | Lat        |  Deg      |  Degrees of Latitude                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 4 | Long       |  Deg      |  Degrees of Longitude                                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 5 | Height     |  m        |  Height in meters                                                     |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 6 | Heading    |  Deg      |  GNSS Heading in Degrees                                              |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 7 | Hacc       |  m        |  Horizontal Accuracy                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 8 | Vacc       |  m        |  Vertical Accuracy                                                    |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 9 | PDOP       |           |                                                                       |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 10| FixType    |           |  GNSS Fix FixType                                                     |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 11| SatNum     |           |  Number of Satellites used in solution                                |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 12| Speed Acc  |           |  Accuracy of GNSS Speed Measurement                                   |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 13| Hdg Acc    |           |  Accuracy of GNSS Heading Measurement                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+


2.3. APINS Navigation Solution Message

  +---+------------+-----------+-----------------------------------------------------------------------+
  |   | Field      |  Units    |  Description                                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 0 | APINS      |           |  Sentence identifier                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 1 | Time       |  ms       |  Time in milli-seconds since power on                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 2 | Status     |           |  Status of Solution                                                   |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 3 | Lat        |  Deg      |  Degrees of Latitude                                                  |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 4 | Long       |  Deg      |  Degrees of Longitude                                                 |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 5 | Height     |  m        |  Height in meters                                                     |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 6 | VN         |  m/s      |  North Velocity in NED Frame                                          |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 7 | VE         |  m/s      |  East Velocity in NED Frame                                           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 8 | VD         |  m/s      |  Down Velocity in NED Frame                                           |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 9 | Roll       |  Deg      |  Roll Angle, rotation about body frame X                              |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 10| Pitch      |  Deg      |  Pitch Angle, rotation about body frame Y                             |
  +---+------------+-----------+-----------------------------------------------------------------------+
  | 11| Heading    |  Deg      |  Heading Angle, rotation about body frame Z                           |
  +---+------------+-----------+-----------------------------------------------------------------------+

3.  RTCM Data Input 
----------------------

Standard RTCM Messages are forwarded to the Anello A-1 to enable the GNSS receivers to reach RTK precision.
The Anello A-1 receives standard RTCM3.3 in MSM format, including MSM4, MSM5, and MSM7 messages.  The 
Anello Python Program provides an NTRIP client which can connect to a standard NTRIP network and forward the
received RTCM messages into the Anello A-1.

4.  Configuration Input - Odometer Aiding 
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

The easiest way to configure the Anello A-1 is with the Anello Python Program.  The A-1 saves all changes made
thru the Anello Python Program to non-volatile flash memory.  This insures that the unit is properly configured when
used in the field.  

To dyanmically configure the unit the protocol is explained below.  The protocol allows for both temporary (RAM)
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


