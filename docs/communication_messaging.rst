Communication & Messaging
===========================

1.  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO Maritime INS:

    1. Serial (RS-232)
    2. UDP (Ethernet)
    3. CAN (NMEA 2000)

1.1 Serial communication Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Default Baud Rate:**
    * 57600

**Data Format**
    * Data Bits: 8
    * Stop Bits: 1
    * Parity: None

1.2 Port Definitions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+-----------------+-------------------------------------------------------------------+------------------------------------------------------+
| Interface       | Supported Protocols                                               | Functions                                            |
+=================+===================================================================+======================================================+
| RS232-1         | Serial, MAVLink, NMEA 0183                                        | Data input / output, configuration, firmware updates |
+-----------------+-------------------------------------------------------------------+------------------------------------------------------+
| RS232-2         | Serial, MAVLink, NMEA 0183                                        | Data input / output, configuration                   |
+-----------------+-------------------------------------------------------------------+------------------------------------------------------+
| Ethernet        | UDP, MAVLink, NMEA 0183                                           | Data input / output, configuration, log downloads    |
+-----------------+-------------------------------------------------------------------+------------------------------------------------------+
| CAN             | NMEA 2000                                                         | Data input / output                                  |
+-----------------+-------------------------------------------------------------------+------------------------------------------------------+


2. Input Messages
---------------------------------

2.1  NMEA 0183 Input Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ANELLO Maritime INS supports standard NMEA 0183 input messages which allow the USV to send in external sensor information, e.g. for speed-aiding. 
ANELLO also has a set of proprietary messages, following the standard NMEA proprietary format with a prefix of “$P”, company code of “AP” (ANELLO Photonics), and the message code.

To configure NMEA 0183 over a serial port, update the following configs (see
`Configure ANELLO Maritime INS <https://docs-a1.readthedocs.io/en/maritime_ins/getting_started_maritimeins.html#configure-anello-maritime-ins>`__
for instructions on changing settings):

``NM0183_CFG`` = ``101`` (RS232-1) **or** ``102`` (RS232-2)

The default baud rate is ``38400``. To change the baud rate use
``SER_TEL1_BAUD`` for RS232-1 or ``SER_TEL2_BAUD`` for RS232-2.

To configure NMEA 0183 over UDP, update the following configs (see
`Configure ANELLO Maritime INS <https://docs-a1.readthedocs.io/en/maritime_ins/getting_started_maritimeins.html#configure-anello-maritime-ins>`__
for instructions on changing settings):

``NMEA_UDP_EN`` = ``1``

The default port is 19551 for input messages and 19550 for output messages.


2.1.1. RPM: Revolutions
""""""""""""""""""""""""

**Message Format**::

    $--RPM,a,x,x.x,x.x,A*hh

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

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x.x        | True Heading, Degrees                                         |
+-------+------------+---------------------------------------------------------------+
| 2     | T          | T = True                                                      |
+-------+------------+---------------------------------------------------------------+
| 3     | x.x        | Magnetic Heading, Degrees                                     |
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

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x.x        | Wind direction magnitude in degrees                           |
+-------+------------+---------------------------------------------------------------+
| 2     | a          | Wind direction Left/Right of bow                              |
+-------+------------+---------------------------------------------------------------+
| 3     | x.x        | Wind Speed in knots                                           |
+-------+------------+---------------------------------------------------------------+
| 4     | N          | N = Knots                                                     |
+-------+------------+---------------------------------------------------------------+
| 5     | x.x        | Wind Speed in m/s                                             |
+-------+------------+---------------------------------------------------------------+
| 6     | M          | M = Meters Per Second                                         |
+-------+------------+---------------------------------------------------------------+
| 7     | x.x        | Wind Speed in km/hr                                           |
+-------+------------+---------------------------------------------------------------+
| 8     | K          | K = Kilometers Per Hour                                       |
+-------+------------+---------------------------------------------------------------+
| 9     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+



2.1.6. GPSCTRL: GPS Control (ANELLO Proprietary)
"""""""""""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $PAPGPSCTRL,x*hh

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | x          | GPS control, “1” = Use GPS (default), “0” = Ignore GPS        |
+-------+------------+---------------------------------------------------------------+
| 2     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+

2.1.7. AUTOCAL: Speed Sensor Auto-Calibration Control (ANELLO Proprietary)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This message starts/stops the Maritime INS speed sensor auto-calibration routine (e.g., for water-speed aiding sensors). The default state is 0 (not in auto-calibration mode).

**Message Format**::

    $PAPAUTOCAL,x*hh

+-------+------------+--------------------------------------------------------------------------+
| Index | Part       | Description                                                              |
+=======+============+==========================================================================+
| 1     | x          | Auto-calibration control: "0" = Off (default), "1" = On (enter auto-cal) |
+-------+------------+--------------------------------------------------------------------------+
| 2     | hh         | Checksum                                                                 |
+-------+------------+--------------------------------------------------------------------------+

Recommended data collection procedure (while x = 1)

- Operate in calm waters with minimal currents/wind/wake.
- Run out-and-back legs along the same line (reciprocal headings).
- Collect data at ≥ 5 different steady speeds spanning the full operating speed range.
- Each out leg and back leg should be at least 30 seconds at a steady speed.
- Repeat each speed at least once (more repeats improves robustness), and avoid aggressive turns during the steady legs.
- When complete, send ``$PAPAUTOCAL,0*hh`` to exit auto-calibration mode.


2.2 NMEA 2000 Input Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The ANELLO Maritime INS also supports the following standard NMEA 2000 input messages, which allow the vehicle to send in external sensor information, e.g. for speed-aiding.

Ensure ``NM2K_CFG`` is set to ``1`` before using the NMEA 2000 driver on the
CAN port. This enables the driver and allows the system to publish and receive
NMEA 2000 messages.


2.2.1 PGN 127488: Engine Parameters, Rapid Update
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides data with a high update rate for a specific engine in a single frame message.

+---+-----------------------+-------------------------------------------------+------+----------------+
| # | Field                 | Description                                     | Unit | Type           |
+===+=======================+=================================================+======+================+
| 1 | Engine Instance       | Identifies the specific engine (0=Single)       |      | 8-bit unsigned |
+---+-----------------------+-------------------------------------------------+------+----------------+
| 2 | Engine Speed          | Engine rotational speed                         | RPM  | 16-bit unsigned|
+---+-----------------------+-------------------------------------------------+------+----------------+
| 3 | Engine Boost Pressure | Turbocharger or supercharger pressure           | kPa  | 16-bit signed  |
+---+-----------------------+-------------------------------------------------+------+----------------+
| 4 | Engine Tilt/Trim      | Engine tilt or trim position                    | %    | 8-bit signed   |
+---+-----------------------+-------------------------------------------------+------+----------------+

Logged topic: NMEA2000_ENGINE

2.2.2 PGN 127489: Engine Parameters, Dynamic
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides real-time operational data and status for a specific engine, usually broadcast periodically for control or instrumentation.

+----+--------------------------+---------------------------------------------+-------+----------------+
| #  | Field                    | Description                                 | Unit  | Type           |
+====+==========================+=============================================+=======+================+
| 1  | Engine Instance          | Identifies the specific engine (0=Single)   |       | 8-bit unsigned |
+----+--------------------------+---------------------------------------------+-------+----------------+
| 2  | Engine Oil Pressure      | Engine lubricant pressure                   | kPa   | 16-bit unsigned|
+----+--------------------------+---------------------------------------------+-------+----------------+
| 3  | Engine Oil Temperature   | Temperature of the engine lubricant         | K     | 16-bit unsigned|
+----+--------------------------+---------------------------------------------+-------+----------------+
| 4  | Engine Temperature       | Temperature of the engine coolant           | K     | 16-bit unsigned|
+----+--------------------------+---------------------------------------------+-------+----------------+
| 5  | Alternator Potential     | Alternator output voltage                   | V     | 16-bit signed  |
+----+--------------------------+---------------------------------------------+-------+----------------+
| 6  | Fuel Rate                | Engine fuel consumption rate                | L/hr  | 16-bit signed  |
+----+--------------------------+---------------------------------------------+-------+----------------+
| 7  | Total Engine Hours       | Cumulative operating time of the engine     | s     | 32-bit unsigned|
+----+--------------------------+---------------------------------------------+-------+----------------+
| 8  | Engine Coolant Pressure  | Pressure of the engine coolant              | kPa   | 16-bit unsigned|
+----+--------------------------+---------------------------------------------+-------+----------------+
| 9  | Fuel Pressure            | Pressure of the fuel                        | kPa   | 16-bit unsigned|
+----+--------------------------+---------------------------------------------+-------+----------------+
| 10 | Engine Discrete Status 1 | Bitmask indicating warnings and statuses    |       | 16-bit bitmap  |
+----+--------------------------+---------------------------------------------+-------+----------------+
| 11 | Engine Discrete Status 2 | Bitmask indicating other statuses           |       | 16-bit bitmap  |
+----+--------------------------+---------------------------------------------+-------+----------------+
| 12 | Percent Engine Load      | Current power output as a percentage of max | %     | 8-bit unsigned |
+----+--------------------------+---------------------------------------------+-------+----------------+
| 13 | Percent Engine Torque    | Current torque output as a percentage of max| %     | 8-bit signed   |
+----+--------------------------+---------------------------------------------+-------+----------------+

Logged topic: NMEA2000_ENGINE_DYN

2.2.3 PGN 128259: Speed, Water Referenced
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides a single transmission describing the motion of a vessel relative to the water.

+---+-----------------------------+----------------------------------------------+------+----------------+
| # | Field                       | Description                                  | Unit | Type           |
+===+=============================+==============================================+======+================+
| 1 | SID                         | Sequence Identifier                          |      | 8-bit unsigned |
+---+-----------------------------+----------------------------------------------+------+----------------+
| 2 | Speed Water Referenced      | Vessel's speed relative to the water         | m/s  | 16-bit signed  |
+---+-----------------------------+----------------------------------------------+------+----------------+
| 3 | Speed Ground Referenced     | Vessel's speed relative to the ground (SOG)  | m/s  | 16-bit signed  |
+---+-----------------------------+----------------------------------------------+------+----------------+
| 4 | Speed Water Referenced Type | Method of measurement (e.g., Paddle wheel)   |      | 8-bit lookup   |
+---+-----------------------------+----------------------------------------------+------+----------------+
| 5 | Speed Direction             | Direction of water-referenced speed          |      | 4-bit unsigned |
+---+-----------------------------+----------------------------------------------+------+----------------+

Logged topic: NMEA2000_SPEED

2.2.4 PGN 128275: Distance Log
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Cumulative voyage distance traveled since last reset, tagged with time and date.

+---+-----------------------------+-----------------------------------------+------+----------------+
| # | Field                       | Description                             | Unit | Type           |
+===+=============================+=========================================+======+================+
| 1 | Date                        | Days since January 1, 1970              | d    | 16-bit unsigned|
+---+-----------------------------+-----------------------------------------+------+----------------+
| 2 | Time                        | Seconds since midnight                  | s    | 32-bit unsigned|
+---+-----------------------------+-----------------------------------------+------+----------------+
| 3 | Total Cumulative Distance   | Total distance traveled through water   | m    | 32-bit unsigned|
+---+-----------------------------+-----------------------------------------+------+----------------+
| 4 | Distance Since Last Reset   | Distance traveled since last reset      | m    | 32-bit unsigned|
+---+-----------------------------+-----------------------------------------+------+----------------+

Logged topic: NMEA2000_DISTANCE

2.2.5 PGN 130311: Environmental Parameters
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

These values provide weather and ambient condition data, often used for sensor calibration, navigation adjustments, and environmental awareness.

+---+------------------------+------------------------------------------+------+----------------+
| # | Field                  | Description                              | Unit | Type           |
+===+========================+==========================================+======+================+
| 1 | SID                    | Sequence Identifier                      |      | 8-bit unsigned |
+---+------------------------+------------------------------------------+------+----------------+
| 2 | Temperature Source     | Source of the temperature reading        |      | 6-bit lookup   |
+---+------------------------+------------------------------------------+------+----------------+
| 3 | Humidity Source        | Source of the humidity reading           |      | 2-bit lookup   |
+---+------------------------+------------------------------------------+------+----------------+
| 4 | Temperature            | Actual temperature reading               | K    | 16-bit signed  |
+---+------------------------+------------------------------------------+------+----------------+
| 5 | Humidity               | Relative humidity                        | %    | 16-bit signed  |
+---+------------------------+------------------------------------------+------+----------------+
| 6 | Atmospheric Pressure   | Barometric pressure                      | Pa   | 16-bit unsigned|
+---+------------------------+------------------------------------------+------+----------------+

Logged topic: NMEA2000_ENVIRONMENT

2.2.6 PGN 130578: Vessel Speed Components
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Accurately describes the speed of a vessel by component vectors.

+---+---------------------------------------+-------------------------------------------------+------+----------------+
| # | Field                                 | Description                                     | Unit | Type           |
+===+=======================================+=================================================+======+================+
| 1 | Longitudinal Speed, Water-referenced  | Forward/aft speed relative to water (surge)     | m/s  | 16-bit signed  |
+---+---------------------------------------+-------------------------------------------------+------+----------------+
| 2 | Transverse Speed, Water-referenced    | Port/starboard speed relative to water (sway)   | m/s  | 16-bit signed  |
+---+---------------------------------------+-------------------------------------------------+------+----------------+
| 3 | Longitudinal Speed, Ground-referenced | Forward/aft speed relative to ground            | m/s  | 16-bit signed  |
+---+---------------------------------------+-------------------------------------------------+------+----------------+
| 4 | Transverse Speed, Ground-referenced   | Port/starboard speed relative to ground         | m/s  | 16-bit signed  |
+---+---------------------------------------+-------------------------------------------------+------+----------------+
| 5 | Stern Speed, Water-referenced         | Transverse speed of the stern relative to water | m/s  | 16-bit signed  |
+---+---------------------------------------+-------------------------------------------------+------+----------------+
| 6 | Stern Speed, Ground-referenced        | Transverse speed of the stern relative to ground| m/s  | 16-bit signed  |
+---+---------------------------------------+-------------------------------------------------+------+----------------+

Logged topic: NMEA2000_VESSEL_SPEED

2.2.7 PGN 65281: GPS Control
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Manufacturer proprietary message used to enable or disable the GPS through the NMEA2000 interface.

+---+------------+----------------------------------+------+----------------+
| # | Field      | Description                      | Unit | Type           |
+===+============+==================================+======+================+
| 1 | GPS Control| GPS enable/disable command:      |      | 8-bit unsigned |
|   |            | 0 = Disable GPS,                 |      |                |
|   |            | 1 = Enable GPS                   |      |                |
+---+------------+----------------------------------+------+----------------+

Logged topic: NMEA2000_GPSCTRL

2.2.8 PGN 65282: Speed Sensor Auto-Calibration Control (Manufacturer Proprietary)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Manufacturer proprietary NMEA 2000 message used to start/stop the speed sensor auto-calibration routine. The default state is 0 (not in auto-calibration mode).

+-------+----------------------------+-----------------------------------------------------------+------+------------------+
| Field | Name                       | Description                                               | Unit | Type             |
+=======+============================+===========================================================+======+==================+
| 1     | Auto-calibration Control   | 0 = Off (default), 1 = On (enter auto-calibration mode)   |      | 8-bit unsigned   |
+-------+----------------------------+-----------------------------------------------------------+------+------------------+

Recommended data collection procedure (while Auto-calibration Control = 1)

- Operate in calm waters with minimal currents/wind/wake.
- Run out-and-back legs on reciprocal headings.
- Use ≥ 5 steady speeds spanning your full speed range.
- Hold each leg ≥ 30 seconds at steady speed.
- Exit auto-calibration by transmitting Auto-calibration Control = 0.

3. Output Messages
-------------------------
3.1 NMEA 0183 Output Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To configure NMEA 0183 over a serial port, update the following configs (see
`Configure ANELLO Maritime INS <https://docs-a1.readthedocs.io/en/maritime_ins/getting_started_maritimeins.html#configure-anello-maritime-ins>`__
for instructions on changing settings):

* ``NM0183_SER_CFG`` = ``101`` (RS232-1) **or** ``102`` (RS232-2)
* ``NM0183_ODR_GGA`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NM0183_ODR_RMC`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)

The default baud rate is ``38400``. To change the baud rate use
``SER_TEL1_BAUD`` for RS232-1 or ``SER_TEL2_BAUD`` for RS232-2.

To configure NMEA 0183 over UDP, update the following configs (see
`Configure ANELLO Maritime INS <https://docs-a1.readthedocs.io/en/maritime_ins/getting_started_maritimeins.html#configure-anello-maritime-ins>`__
for instructions on changing settings):

* ``NMEA_UDP_EN`` = ``1``
* ``NMEA_UDP_ODR_GGA`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NMEA_UDP_ODR_RMC`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)

The default output port is 19550 and input port is 19551

3.1.1 RMC: Recommended Minimum Navigation Information
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--RMC,hhmmss.ss,A,xxxx.xx,a,xxxxx.xx,a,x.x,x.x,xxxx,x.x,a*hh

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

3.1.2 GGA: Global Positioning System Fix Data
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--GGA,hhmmss.ss,llll.ll,a,yyyyy.yy,a,x,xx,x.x,x.x,M,x.x,M,x.x,xxxx*hh

+--------+------------+--------------------------------------------------------------------------+
| Index  | Part       | Description                                                              |
+========+============+==========================================================================+
| 1      | hhmmss.ss  | Time (UTC)                                                               |
+--------+------------+--------------------------------------------------------------------------+
| 2      | llll.ll    | Latitude                                                                 |
+--------+------------+--------------------------------------------------------------------------+
| 3      | a          | N or S                                                                   |
+--------+------------+--------------------------------------------------------------------------+
| 4      | yyyyy.yy   | Longitude                                                                |
+--------+------------+--------------------------------------------------------------------------+
| 5      | a          | E or W                                                                   |
+--------+------------+--------------------------------------------------------------------------+
| 6      | x          | GPS Quality Indicator (0=Invalid; 1=GPS fix; 2=DGPS fix)                 |
+--------+------------+--------------------------------------------------------------------------+
| 7      | xx         | Number of satellites in use (00-12)                                      |
+--------+------------+--------------------------------------------------------------------------+
| 8      | x.x        | Horizontal Dilution of Precision (HDOP)                                  |
+--------+------------+--------------------------------------------------------------------------+
| 9      | x.x        | Altitude (MSL)                                                           |
+--------+------------+--------------------------------------------------------------------------+
| 10     | M          | Units of altitude (M=Meters)                                             |
+--------+------------+--------------------------------------------------------------------------+
| 11     | x.x        | Geoidal separation                                                       |
+--------+------------+--------------------------------------------------------------------------+
| 12     | M          | Units of geoidal separation (M=Meters)                                   |
+--------+------------+--------------------------------------------------------------------------+
| 13     | x.x        | Age of differential data (seconds)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 14     | xxxx       | Differential Reference Station ID (0000-1023)                            |
+--------+------------+--------------------------------------------------------------------------+
| 15     | hh         | Checksum                                                                 |
+--------+------------+--------------------------------------------------------------------------+

3.2 NMEA 2000 Output Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ensure ``NM2K_CFG`` is set to ``1`` before using the NMEA 2000 driver on the
CAN port. This enables the driver and allows the system to publish and receive
NMEA 2000 messages.

Each published PGN has an associated output data rate parameter (for example,
``NM2K_129025_RATE``, ``NM2K_129026_RATE``, ``NM2K_129029_RATE``) within the
**NMEA2000** parameter group. Rates are specified in Hertz, clamped to the
range ``0``–``100``; setting a rate to ``0`` disables that PGN.

3.2.1 PGN 129025: Position, Rapid Update
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

High-speed update of vessel latitude/longitude position.

+---+-------------+----------------------+--------+----------------+
| # | Field       | Description          | Unit   | Type           |
+===+=============+======================+========+================+
| 1 | Latitude    | Position latitude    | deg    | 32-bit signed  |
+---+-------------+----------------------+--------+----------------+
| 2 | Longitude   | Position longitude   | deg    | 32-bit signed  |
+---+-------------+----------------------+--------+----------------+


3.2.2 PGN 129026: COG & SOG, Rapid Update
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Rapid update of Course Over Ground (COG) and Speed Over Ground (SOG).

+---+----------------+--------------------------------+--------+----------------+
| # | Field          | Description                    | Unit   | Type           |
+===+================+================================+========+================+
| 1 | SID            | Sequence Identifier            |        | 8-bit unsigned |
+---+----------------+--------------------------------+--------+----------------+
| 2 | COG Reference  | True/Magnetic reference        |        | 2-bit lookup   |
+---+----------------+--------------------------------+--------+----------------+
| 3 | COG            | Course over ground             | rad    | 16-bit unsigned|
+---+----------------+--------------------------------+--------+----------------+
| 4 | SOG            | Speed over ground              | m/s    | 16-bit unsigned|
+---+----------------+--------------------------------+--------+----------------+


3.2.3 PGN 129029: GNSS Position Data
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Complete GNSS navigation solution including position, quality, and DOP.

+-----+-------------------------+---------------------------------------+------+------------------+
| #   | Field                   | Description                           | Unit | Type             |
+=====+=========================+=======================================+======+==================+
| 1   | SID                     | Sequence Identifier                   |      | 8-bit unsigned   |
+-----+-------------------------+---------------------------------------+------+------------------+
| 2   | Date                    | Days since 1970-01-01                 | days | 16-bit unsigned  |
+-----+-------------------------+---------------------------------------+------+------------------+
| 3   | Time                    | Seconds since midnight                | s    | 32-bit unsigned  |
+-----+-------------------------+---------------------------------------+------+------------------+
| 4   | Latitude                | GNSS latitude (WGS-84)                | deg  | 64-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+
| 5   | Longitude               | GNSS longitude (WGS-84)               | deg  | 64-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+
| 6   | Altitude                | Altitude referenced to WGS-84         | m    | 64-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+
| 7   | GNSS Type               | GPS, GLONASS, Galileo, BeiDou, etc.   |      | 4-bit lookup     |
+-----+-------------------------+---------------------------------------+------+------------------+
| 8   | Method                  | No fix, RTK, etc. (*see table below*) |      | 4-bit lookup     |
+-----+-------------------------+---------------------------------------+------+------------------+
| 9   | Integrity               | Integrity flag                        |      | 2-bit lookup     |
+-----+-------------------------+---------------------------------------+------+------------------+
| 10  | Reserved                | Reserved (transmit as 0)              |      | 6 bits reserved  |
+-----+-------------------------+---------------------------------------+------+------------------+
| 11  | Number of SVs           | Number of satellites used in solution |      | 8-bit unsigned   |
+-----+-------------------------+---------------------------------------+------+------------------+
| 12  | HDOP                    | Horizontal dilution of precision      |      | 16-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+
| 13  | PDOP                    | Positional dilution of precision      |      | 16-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+
| 14  | Geoidal Separation      | Geoid–ellipsoid separation            | m    | 32-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+
| 15  | Reference Stations      | Number of reference stations          |      | 8-bit unsigned   |
+-----+-------------------------+---------------------------------------+------+------------------+
| 16  | Reference Station Type  | Type for reference station #1         |      | 4-bit lookup     |
+-----+-------------------------+---------------------------------------+------+------------------+
| 17  | Reference Station ID    | ID for reference station #1           |      | 12-bit unsigned  |
+-----+-------------------------+---------------------------------------+------+------------------+
| 18  | Age of DGNSS Corrections| Age of corrections for station #1     | s    | 16-bit signed    |
+-----+-------------------------+---------------------------------------+------+------------------+

**Method Field Values (4-bit lookup):**

+------+----------------------------+
| Value| Meaning                    |
+======+============================+
| 0    | No GNSS (no valid fix)     |
+------+----------------------------+
| 1    | GNSS Fix (standard 2D/3D)  |
+------+----------------------------+
| 2    | DGNSS (differential fix)   |
+------+----------------------------+
| 3    | Precise GNSS               |
+------+----------------------------+
| 4    | RTK Fixed Integer          |
+------+----------------------------+
| 5    | RTK Float                  |
+------+----------------------------+
| 6    | Estimated (dead reckoning) |
+------+----------------------------+
| 7    | Manual Input               |
+------+----------------------------+
| 8    | Simulation Mode            |
+------+----------------------------+
| 15   | Invalid                    |
+------+----------------------------+



3.2.4 PGN 127250: Vessel Heading
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides vessel heading and related status.

+---+------------------+-----------------------------------+--------+----------------+
| # | Field            | Description                       | Unit   | Type           |
+===+==================+===================================+========+================+
| 1 | SID              | Sequence Identifier               |        | 8-bit unsigned |
+---+------------------+-----------------------------------+--------+----------------+
| 2 | Heading          | Vessel heading                    | rad    | 16-bit unsigned|
+---+------------------+-----------------------------------+--------+----------------+
| 3 | Deviation        | Magnetic deviation                | rad    | 16-bit signed  |
+---+------------------+-----------------------------------+--------+----------------+
| 4 | Variation        | Magnetic variation                | rad    | 16-bit signed  |
+---+------------------+-----------------------------------+--------+----------------+
| 5 | Reference        | True/Magnetic                     |        | 2-bit lookup   |
+---+------------------+-----------------------------------+--------+----------------+


3.2.5 PGN 127251: Rate of Turn
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides vessel rate of turn information.

+---+---------------+-----------------------------+--------+----------------+
| # | Field         | Description                 | Unit   | Type           |
+===+===============+=============================+========+================+
| 1 | SID           | Sequence Identifier         |        | 8-bit unsigned |
+---+---------------+-----------------------------+--------+----------------+
| 2 | Rate of Turn  | Positive = turn to starboard| rad/s  | 32-bit signed  |
+---+---------------+-----------------------------+--------+----------------+


3.2.6 PGN 127257: Attitude
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides vessel orientation (roll, pitch, yaw).

+---+--------+-------------------------+--------+----------------+
| # | Field  | Description             | Unit   | Type           |
+===+========+=========================+========+================+
| 1 | SID    | Sequence Identifier     |        | 8-bit unsigned |
+---+--------+-------------------------+--------+----------------+
| 2 | Yaw    | Vessel yaw angle        | rad    | 16-bit signed  |
+---+--------+-------------------------+--------+----------------+
| 3 | Pitch  | Vessel pitch angle      | rad    | 16-bit signed  |
+---+--------+-------------------------+--------+----------------+
| 4 | Roll   | Vessel roll angle       | rad    | 16-bit signed  |
+---+--------+-------------------------+--------+----------------+


3.2.7 PGN 126992: System Time
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Provides system time for network synchronization.

+---+----------------+-------------------------------------+--------+----------------+
| # | Field          | Description                         | Unit   | Type           |
+===+================+=====================================+========+================+
| 1 | SID            | Sequence Identifier                 |        | 8-bit unsigned |
+---+----------------+-------------------------------------+--------+----------------+
| 2 | Source         | Time source (GPS, RTC, etc.)        |        | 8-bit lookup   |
+---+----------------+-------------------------------------+--------+----------------+
| 3 | Date           | Days since 1970-01-01               |        | 16-bit unsigned|
+---+----------------+-------------------------------------+--------+----------------+
| 4 | Time           | Seconds since midnight              | s      | 32-bit unsigned|
+---+----------------+-------------------------------------+--------+----------------+
