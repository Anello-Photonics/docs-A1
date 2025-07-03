Communication & Messaging
===========================

1.  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO Maritime INS:
    1. Serial RS232-1 - NMEA 0183 output messaging
    2. Serial RS232-2 - NMEA 0183 input messaging
    3. Ethernet - QGroundControl

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++




2. Input Messages
---------------------------------

2.1  NMEA 0183 Data Input Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ANELLO Maritime INS supports standard NMEA 0183 input messages which allow the USV to send in external sensor information, e.g. for speed-aiding. ANELLO also has a set of proprietary messages, following the standard NMEA proprietary format with a prefix of “$P”, company code of “AP” (ANELLO Photonics), and the message code.

The minimum sensor aiding for the ANELLO Maritime INS is velocity aiding via either a paddle wheel, doppler velocity log (DVL), or another source.

2.1.1. RPM: Revolutions
""""""""""""""""""""""""

**Message Format**::

    $--RPM,a,x,x.x,x.x,A*hh

$--RPM,a\ :sub:`1` \,x\ :sub:`2` \,x.x\ :sub:`3` \,x.x\ :sub:`4` \,A\ :sub:`5` \*hh\ :sub:`6` \  

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | a          | Source; S = Shaft, E = Engine                                 |
+-------+------------+---------------------------------------------------------------+
| 2     | x          | Engine or shaft number                                        |
+-------+------------+---------------------------------------------------------------+
| 3     | x.x        | Speed, Revolutions per minute                                 |
+-------+------------+---------------------------------------------------------------+
| 4     | x.x        | Propeller pitch, % of maximum, "-" means astern               |
+-------+------------+---------------------------------------------------------------+
| 5     | A          | Status, A means data is valid                                 |
+-------+------------+---------------------------------------------------------------+
| 6     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+


2.1.2. RSA: Rudder Sensor Angle
""""""""""""""""""""""""""""""""

**Message Format**::

    $--RSA,x.x,A,x.x,A*hh

$--RSA,x.x\ :sub:`1` \,A\ :sub:`2` \,x.x\ :sub:`3` \,A\ :sub:`4` \*hh\ :sub:`5` \  

+-------+------------+-------------------------------------------------------------+
| Index | Part       | Description                                                 |
+=======+============+=============================================================+
| 1     | x.x        | Starboard (or single) rudder sensor, "-" means Turn To Port |
+-------+------------+-------------------------------------------------------------+
| 2     | A          | Status, A means data is valid                               |
+-------+------------+-------------------------------------------------------------+
| 3     | x.x        | Port rudder sensor                                          |
+-------+------------+-------------------------------------------------------------+
| 4     | A          | Status, A means data is valid                               |
+-------+------------+-------------------------------------------------------------+
| 5     | hh         | Checksum                                                    |
+-------+------------+-------------------------------------------------------------+


2.1.3. VHW: Water Speed & Heading
"""""""""""""""""""""""""""""""""

**Message Format**::

    $--VHW,x.x,T,x.x,M,x.x,N,x.x,K*hh

$--VHW,x.x\ :sub:`1` \,T\ :sub:`2` \,x.x\ :sub:`3` \,M\ :sub:`4` \,x.x\ :sub:`5` \,N\ :sub:`6` \,x.x\ :sub:`7` \,K\ :sub:`8` \*hh\ :sub:`9` \  

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x.x        | Degrees                                                       |
+-------+------------+---------------------------------------------------------------+
| 2     | T          | T = True                                                      |
+-------+------------+---------------------------------------------------------------+
| 3     | x.x        | Degrees Magnetic                                              |
+-------+------------+---------------------------------------------------------------+
| 4     | M          | M = Magnetic                                                  |
+-------+------------+---------------------------------------------------------------+
| 5     | x.x        | Knots (speed of vessel relative to the water)                 |
+-------+------------+---------------------------------------------------------------+
| 6     | N          | N = Knots                                                     |
+-------+------------+---------------------------------------------------------------+
| 7     | x.x        | Kilometers (speed of vessel relative to the water)            |
+-------+------------+---------------------------------------------------------------+
| 8     | K          | K = Kilometres per hour                                       |
+-------+------------+---------------------------------------------------------------+
| 9     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+


2.1.4. VBW: Dual Ground/Water Speed
""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--VBW,x.x,x.x,A,x.x,x.x,A*hh

$--VBW,x.x\ :sub:`1` \,x.x\ :sub:`2` \,A\ :sub:`3` \,x.x\ :sub:`4` \,x.x\ :sub:`5` \,A\ :sub:`6` \*hh\ :sub:`7` \  

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x.x        | Longitudinal water speed, “-“ means astern                    |
+-------+------------+---------------------------------------------------------------+
| 2     | x.x        | Transverse water speed, “-“ means port                        |
+-------+------------+---------------------------------------------------------------+
| 3     | A          | Status, A = Data Valid                                        |
+-------+------------+---------------------------------------------------------------+
| 4     | x.x        | Longitudinal ground speed, “-“ means astern                   |
+-------+------------+---------------------------------------------------------------+
| 5     | x.x        | Transverse ground speed, “-“ means port                       |
+-------+------------+---------------------------------------------------------------+
| 6     | A          | Status, A = Data Valid                                        |
+-------+------------+---------------------------------------------------------------+
| 7     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+


2.1.5. VWR: Relative Wind Speed & Angle
""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--VWR,x.x,a,x.x,N,x.x,M,x.x,K*hh

$--VWR,x.x\ :sub:`1` \,a\ :sub:`2` \,x.x\ :sub:`3` \,N\ :sub:`4` \,x.x\ :sub:`5` \,M\ :sub:`6` \,x.x\ :sub:`7` \,K\ :sub:`8` \*hh\ :sub:`9` \  

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x.x        | Wind direction magnitude in degrees                           |
+-------+------------+---------------------------------------------------------------+
| 2     | a          | Wind direction Left/Right of bow                              |
+-------+------------+---------------------------------------------------------------+
| 3     | x.x        | Speed                                                         |
+-------+------------+---------------------------------------------------------------+
| 4     | N          | N = Knots                                                     |
+-------+------------+---------------------------------------------------------------+
| 5     | x.x        | Speed                                                         |
+-------+------------+---------------------------------------------------------------+
| 6     | M          | M = Meters Per Second                                         |
+-------+------------+---------------------------------------------------------------+
| 7     | x.x        | Speed                                                         |
+-------+------------+---------------------------------------------------------------+
| 8     | K          | K = Kilometers Per Hour                                       |
+-------+------------+---------------------------------------------------------------+
| 9     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+



2.1.6. $PAPGPSCTRL: GPS Control
"""""""""""""""""""""""""""""""""

**Message Format**::

    $PAPGPSCTRL,x*hh

$PAPGPSCTRL,x\ :sub:`1` \*hh\ :sub:`2` \  

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x          | GPS control, “1” = Use GPS (default), “0” = Ignore GPS        |
+-------+------------+---------------------------------------------------------------+
| 2     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+



2.2. ANELLO Custom Binary Sensor Input Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In addition to standard NMEA messages, the ANELLO Maritime INS supports a custom binary input message which can be used to populate available sensor information from an external GPS, a paddle wheel sensor, an external magnetometer, a wind speed and direction, and motor and rudder percentage information. This message is detailed below. 
 
**Serial communication protocol0**: RS-232 

**Baud rate**: 115200 (8 data bits, 1 stop bit, no parity, no hardware flow control) (other baud rates available upon request) 

**Transmission rate**: Up to 10 Hz (4 Hz default) 

**Endianness**: All fields are big endian 


The following table shows the format of the sensor data message. 

.. note::
    In the case of no GPS fix / no IMU data/ etc, any invalid data will be set to the max value for its data type. For unsigned types: 0xFF, 0xFFFF, etc. For signed types; 0x7F, 0x7FFF, etc. 

+--------+----------+------------------------------+--------------------------------------------------+
| Offset | Type     | Item                         | Description                                      |
|        |          |                              |                                                  |
+========+==========+==============================+==================================================+
| 0      | Uint16   | Msg ID                       | 0xAB00                                           |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 2      | Uint16   | Msg Length                   | Number of message bytes after CRC                |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 4      | Uint32   | CRC                          | CRC-32 of message payload (bytes 8-N)            |
|        |          |                              | (polynomial 0xEDB88320, starting value 0xFFFFFFF |
+--------+----------+------------------------------+--------------------------------------------------+
| 8      | Uint16   | IMU Compass Heading          | Degrees: 0-360                                   |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 10     | Int32    | GPS Latitude                 | Millionths of degrees                            |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 14     | Int32    | GPS Longitude                | Millionths of degrees                            |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 18     | Uint16   | GPS SOG (speed over ground)  | Tenths of meters per second                      |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 20     | Uint16   | GPS COG (course over ground) | Degrees: 0-360                                   |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 22     | Uint64   | GPS time                     | Milliseconds since epoch (1970)                  |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 30     | Int32    | GPS altitude: MSL            | Tenths of meters                                 |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 34     | Int32    | GPS altitude: geoid separat. | Tenths of meters                                 |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 38     | Uint8    | GPS HDOP                     | Tenths of HDOP (HDOP range 0-1; this field range |
|        |          |                              | 0-10)                                            |
+--------+----------+------------------------------+--------------------------------------------------+
| 39     | Uint8    | GPS fix quality              | GPS Quality indicator (as defined by GPGGA msg)  |
|        |          |                              | 0: Fix not valid   1: GPS FIX                    |
+--------+----------+------------------------------+--------------------------------------------------+
| 40     | Int8     | Motor %                      | [-100, 100]                                      |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 41     | Int8     | Rudder %                     | [-100,100]                                       |
|        |          |                              | 100 = full starboard                             |
+--------+----------+------------------------------+--------------------------------------------------+
| 42     | Uint16   | Speed through water          | Tenths of meters per second                      |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 44     | Uint16   | Wind speed: absolute         | Tenths of meters per second                      |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 46     | Uint16   | Wind dir: absolute           | Degrees: 0-360                                   |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 48     | Uint16   | Wind speed: relative to boat | Tenths of meters per second                      |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 50     | Uint16   | Wind dir: relative to boat   | Degrees: 0-360                                   |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 52     | Int16    | Air temp                     | Tenths of degrees C                              |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+
| 54     | Uint16   | Barometric pressure          | hPa (hectopascal; 1 hPa = 100 Pa)                |
|        |          |                              |                                                  |
+--------+----------+------------------------------+--------------------------------------------------+



2.2.1 Example Data
""""""""""""""""""""

Example Data (mix of valid and invalid data): 

0xAB0000308EF7F176005901F4ECFFF903369A0000FFFF00000190794363D0000001E07FFFFFFFFFFF000000000002006C0003005AFFFFFFFF 

 

0xAB00: header bytes 

0x0030: msg payload length (48 bytes) 

0x8EF7F176: msg crc 

0x0059: IMU compass heading (89 degrees) 

0x01F4ECFF: GPS latitude (32.828671 degrees) 

0xF903369A: GPS longitude (-117.229926 degrees) 

0x0000: GPS SOG (0 mps) 

0xFFFF: GPS COG (invalid/not reported) 

0x00000190794363D0: GPS time (Jul 03 2024 15:42:58:000) 

0x000001E0: GPS altitude: MSL (48m) 

0x7FFFFFFF: GPS altitude: geoid separation (invalid/not reported) 

0xFF: GPS HDOP (invalid/not reported) 

0xFF: GPS fix quality (invalid/not reported) 

0x00: Motor % (0%) 

0x00: Rudder % (0%) 

0x0000: Speed through water (0 mps) 

0x0002: Absolute wind speed (0.2 mps) 

0x006C: Absolute wind direction (108 degrees) 

0x0003: Relative wind speed (0.3 mps) Relative wind direction (90 degrees) 

0xFFFF: Air temperature (invalid / not reported) 

0xFFFF: Barometric pressure (invalid / not reported) 


2.3 NMEA 2000 Data Input Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ANELLO Maritime INS also supports the following standard NMEA 2000 input messages which allow the USV to send in external sensor information, e.g. for speed-aiding.

2.3.1 PGN 127488: NMEA2000_ENGINE
"""""""""""""""""""""""""""""""""""
**Engine Parameters, Rapid Update**

Provides data with a high update rate for a specific engine in a single frame message. The first field provides information as to which engine.

+----+------------------------+
| #  | Field                  |
+====+========================+
| 1  | Engine Instance        |
+----+------------------------+
| 2  | Engine Speed           |
+----+------------------------+
| 3  | Engine Boost Pressure  |
+----+------------------------+
| 4  | Engine Tilt/Trim       |
+----+------------------------+
| 5  | Reserved Bits          |
+----+------------------------+

2.3.2 PGN 127489: NMEA2000_ENGINE_DYN
"""""""""""""""""""""""""""""""""""""""
**Engine Parameters, Dynamic**

Provides real-time operational data and status for a specific engine, usually broadcast periodically for control or instrumentation.

+-----+----------------------------+
| #   | Field                      |
+=====+============================+
| 1   | Engine Instance            |
+-----+----------------------------+
| 2   | Engine Oil Pressure        |
+-----+----------------------------+
| 3   | Engine Oil Temperature     |
+-----+----------------------------+
| 4   | Engine Temperature         |
+-----+----------------------------+
| 5   | Alternator Potential       |
+-----+----------------------------+
| 6   | Fuel Rate                  |
+-----+----------------------------+
| 7   | Total Engine Hours         |
+-----+----------------------------+
| 8   | Engine Coolant Pressure    |
+-----+----------------------------+
| 9   | Fuel Pressure              |
+-----+----------------------------+
| 10  | Not Available              |
+-----+----------------------------+
| 11  | Engine Discrete Status 1   |
+-----+----------------------------+
| 12  | Engine Discrete Status 2   |
+-----+----------------------------+
| 13  | Percent Engine Load        |
+-----+----------------------------+
| 14  | Percent Engine Torque      |
+-----+----------------------------+

2.3.3 PGN 128259: NMEA2000_SPEED
"""""""""""""""""""""""""""""""""
**Speed, Water Referenced**

Provides a single transmission describing the motion of a vessel.

+----+-----------------------------+
| #  | Field                       |
+====+=============================+
| 1  | Sequence ID                 |
+----+-----------------------------+
| 2  | Speed Water Referenced      |
+----+-----------------------------+
| 3  | Speed Ground Referenced     |
+----+-----------------------------+
| 4  | Speed Water Referenced Type |
+----+-----------------------------+
| 5  | Reserved Bits               |
+----+-----------------------------+

2.3.4 PGN 128275: NMEA2000_DISTANCE
"""""""""""""""""""""""""""""""""""""
**Distance Log**

Cumulative voyage distance traveled since last reset, tagged with time and date.

+----+-----------------------------+
| #  | Field                       |
+====+=============================+
| 1  | Measurement Date            |
+----+-----------------------------+
| 2  | Measurement Time            |
+----+-----------------------------+
| 3  | Total Cumulative Distance   |
+----+-----------------------------+
| 4  | Distance Since Last Reset   |
+----+-----------------------------+

2.3.5 PGN 130311: NMEA2000_ENVIRONMENT
"""""""""""""""""""""""""""""""""""""""
**Environmental Parameters**
These values provide weather and ambient condition data, often used for sensor calibration, navigation adjustments, and environmental awareness.

+----+------------------------+
| #  | Field                  |
+====+========================+
| 1  | Sequence ID            |
+----+------------------------+
| 2  | Temperature Instance   |
+----+------------------------+
| 3  | Humidity Instance      |
+----+------------------------+
| 4  | Temperature            |
+----+------------------------+
| 5  | Humidity               |
+----+------------------------+
| 6  | Atmospheric Pressure   |
+----+------------------------+

2.3.6 PGN 130578: NMEA2000_VESSEL_SPEED
""""""""""""""""""""""""""""""""""""""""
**Vessel Speed Components**

Accurately describes the speed of a vessel by component vectors.

+----+--------------------------------------------+
| #  | Field                                      |
+====+============================================+
| 1  | Longitudinal Speed, Water-referenced       |
+----+--------------------------------------------+
| 2  | Transverse Speed, Water-referenced         |
+----+--------------------------------------------+
| 3  | Longitudinal Speed, Ground-referenced      |
+----+--------------------------------------------+
| 4  | Transverse Speed, Ground-referenced        |
+----+--------------------------------------------+
| 5  | Stern Speed, Water-referenced              |
+----+--------------------------------------------+
| 6  | Stern Speed, Ground-referenced             |
+----+--------------------------------------------+





3. Output Messages
-------------------------
*Additional messages available upon request*

3.1 RMC: Recommended Minimum Navigation Information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Message Format**::

    $--RMC,hhmmss.ss,A,xxxx.xx,a,xxxxx.xx,a,x.x,x.x,xxxx,x.x,a*hh

$--RMC,hhmmss.ss\ :sub:`1` \,A\ :sub:`2` \,xxxx.xx\ :sub:`3` \,a\ :sub:`4` \,xxxxx.xx\ :sub:`5` \,a\ :sub:`6` \,x.x\ :sub:`7` \,x.x\ :sub:`8` \,xxxx\ :sub:`9` \,x.x\ :sub:`10` \,a\ :sub:`11` \*hh\ :sub:`12` \  

+--------+------------+--------------------------------------------------------------------------+
| Index  | Part       | Description                                                              |
+========+============+==========================================================================+
| 1      | hhmmss.ss  | Time (UTC)                                                               |
+--------+------------+--------------------------------------------------------------------------+
| 2      | A          | Status, A = Active, V = Navigation receiver warning                      |
+--------+------------+--------------------------------------------------------------------------+
| 3      | xxxx.xx    | Latitude                                                                 |
+--------+------------+--------------------------------------------------------------------------+
| 4      | a          | N or S                                                                   |
+--------+------------+--------------------------------------------------------------------------+
| 5      | xxxxx.xx   | Longitude                                                                |
+--------+------------+--------------------------------------------------------------------------+
| 6      | a          | E or W                                                                   |
+--------+------------+--------------------------------------------------------------------------+
| 7      | x.x        | Speed over ground, knots                                                 |
+--------+------------+--------------------------------------------------------------------------+
| 8      | x.x        | Track made good, degrees true                                            |
+--------+------------+--------------------------------------------------------------------------+
| 9      | xxxx       | Date, ddmmyy                                                             |
+--------+------------+--------------------------------------------------------------------------+
| 10     | x.x        | Magnetic Variation, degrees                                              |
+--------+------------+--------------------------------------------------------------------------+
| 11     | a          | E or W                                                                   |
+--------+------------+--------------------------------------------------------------------------+
| 12     | hh         | Checksum                                                                 |
+--------+------------+--------------------------------------------------------------------------+
