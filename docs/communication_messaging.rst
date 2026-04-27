Communication & Messaging
===========================

1.  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO Aerial INS:

    1. Serial (RS-232)
    2. UDP (Ethernet)
    3. CAN (NMEA 2000)

1.1 Serial Communication Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Default Baud Rate:**
    * RS-232-1: 57600 
    * RS-232-2: 921600 *units shipped prior to 2/19/2026 have a default baud rate of 57600*

The maximum supported baud rate for both serial ports is 921600

**Data Format**
    * Data Bits: 8
    * Stop Bits: 1
    * Parity: None

1.2 Port Definitions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+-----------------+-------------------------------------------------------------------+-----------------------------------------------+------------------------------------------------------+
| Interface       | Supported Protocols                                               | Default Configuration                         | Functions                                            |
+=================+===================================================================+===============================================+======================================================+
| RS232-1         | Serial, MAVLink, NMEA 0183                                        | MAVLink at 57600                              | Data input / output, configuration, firmware updates |
+-----------------+-------------------------------------------------------------------+-----------------------------------------------+------------------------------------------------------+
| RS232-2         | Serial, MAVLink, NMEA 0183                                        | NMEA 0183 at 921600                           | Data input / output, configuration                   |
+-----------------+-------------------------------------------------------------------+-----------------------------------------------+------------------------------------------------------+
| Ethernet        | UDP, MAVLink, NMEA 0183                                           | MAVLink (UDP 14550)                           | Data input / output, configuration, log downloads    |
+-----------------+-------------------------------------------------------------------+-----------------------------------------------+------------------------------------------------------+
| CAN             | NMEA 2000                                                         | NMEA 2000                                     | Data input / output                                  |
+-----------------+-------------------------------------------------------------------+-----------------------------------------------+------------------------------------------------------+

1.3 Time Synchronization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The ANELLO Aerial INS supplies the following I/O pins for time synchronization

+-----------------+--------------------------+------------------------------------------------------+
| Interface       | Max Voltage              | Functions                                            |
+=================+==========================+======================================================+
| PPS             | 3.3 V                    | GNSS time synchronization output pulse               |
+-----------------+--------------------------+------------------------------------------------------+
| Sync            | 3.3 V                    | GNSS time synchronization input pulse                |
+-----------------+--------------------------+------------------------------------------------------+
| Reset           | 3.3 V                    | Driving low restarts the Aerial INS                  |
+-----------------+--------------------------+------------------------------------------------------+

See `Mechanicals <https://docs-a1.readthedocs.io/en/aerial_ins/mechanicals.html>`_ to find the specified output pins.


2. Input Messages
---------------------------------

2.1  NMEA 0183 Input Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ANELLO Aerial INS supports standard NMEA 0183 input messages which allow the UAV to send in external sensor information, e.g. for speed-aiding. 
ANELLO also has a set of proprietary messages, following the standard NMEA proprietary format with a prefix of “$P”, company code of “AP” (ANELLO Photonics), and the message code.

To configure NMEA 0183 over a serial port, update the following configs (see
`Configure ANELLO Aerial INS <https://docs-a1.readthedocs.io/en/aerial_ins/getting_started_aerialins.html#configure-anello-aerial-ins>`__
for instructions on changing settings):

``NM0183_CFG`` = ``1`` (RS232-1) **or** ``2`` (RS232-2)

To change the baud rate use ``SER_TEL1_BAUD`` for RS232-1 or ``SER_TEL2_BAUD`` for RS232-2.

For the full table of serial NMEA0183 parameters, see :ref:`nmea0183-serial-parameters`.

To configure NMEA 0183 over UDP, update the following configs (see
`Configure ANELLO Aerial INS <https://docs-a1.readthedocs.io/en/aerial_ins/getting_started_aerialins.html#configure-anello-aerial-ins>`__
for instructions on changing settings):

``NMUDP_EN`` = ``1``

UDP input uses port ``19551`` and UDP output uses port ``19550``.
External UDP output only occurs when ``NMUDP_MC_IP0`` through ``NMUDP_MC_IP3``
define a valid multicast group. Setting ``NMUDP_EN`` to ``1`` alone does not
produce external UDP output.

See :ref:`nmea0183-over-udp-parameters` for the full parameter table.


2.1.1 External Sensor Aiding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

2.1.1.1. VHW: Water Speed & Heading
""""""""""""""""""""""""""""""""""""

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
| 7     | x.x        | Kilometers per hour (speed of vessel relative to the water)   |
+-------+------------+---------------------------------------------------------------+
| 8     | K          | K = Kilometres per hour                                       |
+-------+------------+---------------------------------------------------------------+
| 9     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+

.. note::
   Aerial INS uses fields 1, 3, 5, and 7 from this sentence.


2.1.1.2. VBW: Dual Ground/Water Speed
"""""""""""""""""""""""""""""""""""""""

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

.. note::
   Aerial INS uses fields 1 through 6 from this sentence.


2.1.2 External Position Aiding
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
To enable external NMEA0183 GNSS input set ``EKF2_GPS_EXT_EN`` = ``1`` or ``Enabled``

To enable a secondary input-only serial port to receive external NMEA0183 GNSS input set ``NM_GNSS_CFG`` = ``1`` (RS232-1) **or** ``2`` (RS232-2)

To use an external GNSS input, the minimum required messages are ``GGA``, ``RMC``, and ``GSA``
at a rate of at least ``0.5 Hz``. In order for the algorithm to use external GNSS
updates, ``RMC`` must report ``status = A``, latitude and longitude must be valid,
``GSA`` mode must be ``3`` or greater,
and PDOP, HDOP, and VDOP must all be present, finite, and greater than zero.

See :ref:`external-position-aiding-parameters` for the parameter table used to configure external position aiding.
See `Configure ANELLO Aerial INS <https://docs-a1.readthedocs.io/en/aerial_ins/getting_started_aerialins.html#configure-anello-aerial-ins>`__
for instructions on changing settings.

2.1.2.1. RMC: Recommended Minimum Navigation Information
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--RMC,hhmmss.ss,A,ddmm.mmmmmm,a,dddmm.mmmmmm,a,x.x,x.x,xxxx,x.x,a*hh

+--------+-------------+--------------------------------------------------------------------------+
| Index  | Part        | Description                                                              |
+========+=============+==========================================================================+
| 1      | hhmmss.ss   | Time (UTC)                                                               |
+--------+-------------+--------------------------------------------------------------------------+
| 2      | A           | Status, A = Active, V = Navigation receiver warning                      |
+--------+-------------+--------------------------------------------------------------------------+
| 3      | ddmm.mmmmmm | Latitude                                                                 |
+--------+-------------+--------------------------------------------------------------------------+
| 4      | a           | N or S                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 5      | dddmm.mmmmmm| Longitude                                                                |
+--------+-------------+--------------------------------------------------------------------------+
| 6      | a           | E or W                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 7      | x.x         | Speed over ground, knots                                                 |
+--------+-------------+--------------------------------------------------------------------------+
| 8      | x.x         | Track made good, degrees true                                            |
+--------+-------------+--------------------------------------------------------------------------+
| 9      | xxxx        | Date, ddmmyy                                                             |
+--------+-------------+--------------------------------------------------------------------------+
| 10     | x.x         | Magnetic Variation, degrees                                              |
+--------+-------------+--------------------------------------------------------------------------+
| 11     | a           | E or W                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 12     | hh          | Checksum                                                                 |
+--------+-------------+--------------------------------------------------------------------------+

.. note::
   Aerial INS uses fields 1 through 9 for external GNSS input. Fields 10 and
   11 are logged but are not used in the real-time algorithm.

2.1.2.2. GGA: Global Positioning System Fix Data
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--GGA,hhmmss.ss,ddmm.mmmmmm,a,dddmm.mmmmmm,a,x,xx,x.x,x.x,M,x.x,M,x.x,xxxx*hh

+--------+-------------+--------------------------------------------------------------------------+
| Index  | Part        | Description                                                              |
+========+=============+==========================================================================+
| 1      | hhmmss.ss   | Time (UTC)                                                               |
+--------+-------------+--------------------------------------------------------------------------+
| 2      | ddmm.mmmmmm | Latitude                                                                 |
+--------+-------------+--------------------------------------------------------------------------+
| 3      | a           | N or S                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 4      | dddmm.mmmmmm| Longitude                                                                |
+--------+-------------+--------------------------------------------------------------------------+
| 5      | a           | E or W                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 6      | x           | GPS Quality Indicator (see table below)                                  |
+--------+-------------+--------------------------------------------------------------------------+
| 7      | xx          | Number of satellites in use (00-12)                                      |
+--------+-------------+--------------------------------------------------------------------------+
| 8      | x.x         | Horizontal Dilution of Precision (HDOP)                                  |
+--------+-------------+--------------------------------------------------------------------------+
| 9      | x.x         | Altitude (MSL)                                                           |
+--------+-------------+--------------------------------------------------------------------------+
| 10     | M           | Units of altitude (M=Meters)                                             |
+--------+-------------+--------------------------------------------------------------------------+
| 11     | x.x         | Geoidal separation                                                       |
+--------+-------------+--------------------------------------------------------------------------+
| 12     | M           | Units of geoidal separation (M=Meters)                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 13     | x.x         | Age of differential data (seconds)                                       |
+--------+-------------+--------------------------------------------------------------------------+
| 14     | xxxx        | Differential Reference Station ID (0000-1023)                            |
+--------+-------------+--------------------------------------------------------------------------+
| 15     | hh          | Checksum                                                                 |
+--------+-------------+--------------------------------------------------------------------------+

.. note::
   Aerial INS uses fields 1 through 9 and field 11 from this sentence.

**GPS Quality Indicator**

+-------+------------------------------------------------------------------+
| Value | Description                                                      |
+=======+==================================================================+
| 0     | Fix not available or invalid                                     |
+-------+------------------------------------------------------------------+
| 1     | GPS fix (no corrections)                                         |
+-------+------------------------------------------------------------------+
| 2     | Differential GPS fix                                             |
+-------+------------------------------------------------------------------+
| 4     | RTK Fixed                                                        |
+-------+------------------------------------------------------------------+
| 5     | RTK Float                                                        |
+-------+------------------------------------------------------------------+
| 6     | Dead reckoning mode (GPS is determined to be jammed or spoofed)  |
+-------+------------------------------------------------------------------+


2.1.2.3. GSA: GNSS DOP and Active Satellites
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

**Message Format**::

    $--GSA,a,x,xx,xx,xx,xx,xx,xx,xx,xx,xx,xx,xx,xx,xx,x.x,x.x,x.x*hh

+--------+------------+--------------------------------------------------------------------------+
| Index  | Part       | Description                                                              |
+========+============+==========================================================================+
| 1      | a          | Mode 1: M = Manual, A = Automatic                                        |
+--------+------------+--------------------------------------------------------------------------+
| 2      | x          | Mode 2: Fix Type (1 = No Fix, 2 = 2D Fix, 3 = 3D Fix)                    |
+--------+------------+--------------------------------------------------------------------------+
| 3      | xx         | Satellite ID used for fix (PRN #1)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 4      | xx         | Satellite ID used for fix (PRN #2)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 5      | xx         | Satellite ID used for fix (PRN #3)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 6      | xx         | Satellite ID used for fix (PRN #4)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 7      | xx         | Satellite ID used for fix (PRN #5)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 8      | xx         | Satellite ID used for fix (PRN #6)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 9      | xx         | Satellite ID used for fix (PRN #7)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 10     | xx         | Satellite ID used for fix (PRN #8)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 11     | xx         | Satellite ID used for fix (PRN #9)                                       |
+--------+------------+--------------------------------------------------------------------------+
| 12     | xx         | Satellite ID used for fix (PRN #10)                                      |
+--------+------------+--------------------------------------------------------------------------+
| 13     | xx         | Satellite ID used for fix (PRN #11)                                      |
+--------+------------+--------------------------------------------------------------------------+
| 14     | xx         | Satellite ID used for fix (PRN #12)                                      |
+--------+------------+--------------------------------------------------------------------------+
| 15     | x.x        | PDOP (Position Dilution of Precision)                                    |
+--------+------------+--------------------------------------------------------------------------+
| 16     | x.x        | HDOP (Horizontal Dilution of Precision)                                  |
+--------+------------+--------------------------------------------------------------------------+
| 17     | x.x        | VDOP (Vertical Dilution of Precision)                                    |
+--------+------------+--------------------------------------------------------------------------+
| 18     | hh         | Checksum                                                                 |
+--------+------------+--------------------------------------------------------------------------+

.. note::
   Aerial INS uses fields 1 through 17 from this sentence. The real-time
   algorithm relies on fields 2, 15, 16, and 17.


2.1.3 ANELLO Proprietary
^^^^^^^^^^^^^^^^^^^^^^^^^^^^


2.1.3.1. GPSCTRL: GPS Control (ANELLO Proprietary)
""""""""""""""""""""""""""""""""""""""""""""""""""""

Enables or disables GPS utilization in sensor fusion algorithm.

**Message Format**::

    $PAPGPSCTRL,A*hh

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | A          | GPS control, “1” = Use GPS (default), “0” = Ignore GPS        |
+-------+------------+---------------------------------------------------------------+
| 2     | hh         | Checksum                                                      |
+-------+------------+---------------------------------------------------------------+

.. note::
   Aerial INS uses field 1 from this sentence.

2.1.3.2. AUTOCAL: Speed Sensor Auto-Calibration Control (ANELLO Proprietary)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This message starts/stops the Aerial INS speed sensor auto-calibration routine (e.g., for water-speed aiding sensors). The default state is 0 (not in auto-calibration mode).

**Message Format**::

    $PAPAUTOCAL,A*hh

+-------+------------+--------------------------------------------------------------------------+
| Index | Part       | Description                                                              |
+=======+============+==========================================================================+
| 1     | A          | Auto-calibration control: "0" = Off (default), "1" = On (enter auto-cal) |
+-------+------------+--------------------------------------------------------------------------+
| 2     | hh         | Checksum                                                                 |
+-------+------------+--------------------------------------------------------------------------+

.. note::
   Aerial INS uses field 1 from this sentence.

Recommended data collection procedure (while x = 1)

- Operate in calm waters with minimal currents/wind/wake.
- Run out-and-back legs along the same line (reciprocal headings).
- Collect data at ≥ 5 different steady speeds spanning the full operating speed range.
- Each out leg and back leg should be at least 30 seconds at a steady speed.
- Repeat each speed at least once (more repeats improves robustness), and avoid aggressive turns during the steady legs.
- When complete, send ``$PAPAUTOCAL,0*hh`` to exit auto-calibration mode.


2.1.3.3. PAPPOS: Auxiliary Position (Lat/Lon/Alt) (ANELLO Proprietary) 
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
This message can be used to pass in an external position, either from user input or an external aiding source such as visual waypoint detection, USBL acoustic positioning, star tracker, or an M Code receiver.

**Message Format**::

    $PAPPOS,hhmmss.ss,PX,PY,PZ,H_acc,V_acc*hh


+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | hhmmss.ss  | UTC time field. This field may be left empty, but the comma   |
|       |            | position is required.                                         |
+-------+------------+---------------------------------------------------------------+
| 2     | PX         | Latitude in degrees (signed; +N / -S)                         |
+-------+------------+---------------------------------------------------------------+
| 3     | PY         | Longitude in degrees (signed; +E / -W)                        |
+-------+------------+---------------------------------------------------------------+
| 4     | PZ         | Altitude above mean sea level (meters)                        |
+-------+------------+---------------------------------------------------------------+
| 5     | H_acc      | Horizontal accuracy / uncertainty (meters)                    |
+-------+------------+---------------------------------------------------------------+
| 6     | V_acc      | Vertical accuracy / uncertainty (meters)                      |
+-------+------------+---------------------------------------------------------------+
| 7     | hh         | NMEA checksum (hex)                                           |
+-------+------------+---------------------------------------------------------------+

.. note::
   Aerial INS uses fields 2 through 6 from this sentence.

2.1.3.4. PAPRPH: Roll/Pitch/Heading (with Accuracies) (ANELLO Proprietary) 
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This message can be used to pass in an external heading, either from user input or an external aiding source such as a well-calibrated magnetometer, star tracker, or an M Code receiver. *Currently only external heading aiding is implemented, and roll/pitch aiding are available upon request.*

**Message Format**::

    $PAPRPH,hhmmss.ss,R,P,Y,R_acc,P_acc,H_acc*hh


+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | hhmmss.ss  | UTC time field. This field may be left empty, but the comma   |
|       |            | position is required.                                         |
+-------+------------+---------------------------------------------------------------+
| 2     | R          | Roll angle (degrees)                                          |
+-------+------------+---------------------------------------------------------------+
| 3     | P          | Pitch angle (degrees)                                         |
+-------+------------+---------------------------------------------------------------+
| 4     | Y          | Heading / yaw (degrees, typically 0..360)                     |
+-------+------------+---------------------------------------------------------------+
| 5     | R_acc      | Roll accuracy / uncertainty (degrees)                         |
+-------+------------+---------------------------------------------------------------+
| 6     | P_acc      | Pitch accuracy / uncertainty (degrees)                        |
+-------+------------+---------------------------------------------------------------+
| 7     | H_acc      | Heading accuracy / uncertainty (degrees)                      |
+-------+------------+---------------------------------------------------------------+
| 8     | hh         | NMEA checksum (hex)                                           |
+-------+------------+---------------------------------------------------------------+

.. note::
   Aerial INS uses fields 2 through 7 from this sentence.

2.1.3.5. APMAV: Restore Serial MAVLink Access (ANELLO Proprietary)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

This recovery command enables MAVLink on RS232-1, disables the NMEA0183 serial
driver, and reboots the unit. This is intended if you only have serial
connected to your computer and you need to change parameters, which is done
over MAVLink.

**Message Format**::

    $APMAV*hh

+-------+------------+---------------------------------------------------------------+
| Index | Part       | Description                                                   |
+=======+============+===============================================================+
| 1     | hh         | NMEA checksum (hex)                                           |
+-------+------------+---------------------------------------------------------------+





2.2 NMEA 2000 Input Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The ANELLO Aerial INS also supports the following standard NMEA 2000 input messages, which allow the vehicle to send in external sensor information, e.g. for speed-aiding.

Ensure ``NM2K_CFG`` is set to ``1`` before using the NMEA 2000 driver on the
CAN port. This enables the driver and allows the system to publish and receive
NMEA 2000 messages.


2.2.1 PGN 127488: Engine Parameters, Rapid Update
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Aerial INS uses fields 1 through 4 from this PGN.

Logged topic: NMEA2000_ENGINE

2.2.2 PGN 127489: Engine Parameters, Dynamic
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Aerial INS uses fields 1 through 13 from this PGN. Fields 10 and 11 are
   logged as raw bitmaps.

Logged topic: NMEA2000_ENGINE_DYN

2.2.3 PGN 128259: Speed, Water Referenced
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Aerial INS uses fields 1 through 5 from this PGN. Fields 4 and 5 are used
   without remapping.

Logged topic: NMEA2000_SPEED

2.2.4 PGN 128275: Distance Log
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Aerial INS uses fields 1 through 4 from this PGN.

Logged topic: NMEA2000_DISTANCE

2.2.5 PGN 130311: Environmental Parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Aerial INS uses fields 1 through 6 from this PGN. Fields 2 and 3 are used
   without remapping.

Logged topic: NMEA2000_ENVIRONMENT

2.2.6 PGN 130578: Vessel Speed Components
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Aerial INS uses fields 1 through 6 from this PGN.

Logged topic: NMEA2000_VESSEL_SPEED

2.2.7 PGN 65281: GPS Control
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ANELLO proprietary message used to enable or disable the GPS through the NMEA2000 interface.

+---+------------+----------------------------------+------+----------------+
| # | Field      | Description                      | Unit | Type           |
+===+============+==================================+======+================+
| 1 | GPS Control| GPS enable/disable command:      |      | 8-bit unsigned |
|   |            | 0 = Disable GPS,                 |      |                |
|   |            | 1 = Enable GPS                   |      |                |
+---+------------+----------------------------------+------+----------------+

.. note::
   Aerial INS uses field 1 from this PGN.

Logged topic: NMEA2000_GPSCTRL


2.2.8 PGN 65282: Speed Sensor Auto-Calibration Control (ANELLO Proprietary)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ANELLO proprietary NMEA 2000 message used to start/stop the speed sensor auto-calibration routine. The default state is 0 (not in auto-calibration mode).

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

.. note::
   Aerial INS uses field 1 from this PGN.

Logged topic: NMEA2000_AUTOCAL_WATERSPEED

2.2.9 PGN 127493: Transmission Parameters, Dynamic
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Provides real-time operational data and status for a specific transmission, typically broadcast periodically for monitoring and instrumentation.

+----+----------------------+-----------------------------------------------+-------+------------------+
| #  | Field                | Description                                   | Unit  | Type             |
+====+======================+===============================================+=======+==================+
| 1  | Transmission Instance| Identifies the specific transmission instance |       | 8-bit unsigned   |
+----+----------------------+-----------------------------------------------+-------+------------------+
| 2  | Gear*                |  Current transmission gear state              |       | 2-bit unsigned   |
+----+----------------------+-----------------------------------------------+-------+------------------+
| 3  | Reserved             | Reserved for future use                       |       | 6-bit unsigned   |
+----+----------------------+-----------------------------------------------+-------+------------------+
| 4  | Oil Pressure         | Transmission oil pressure                     | kPa   | 16-bit unsigned  |
+----+----------------------+-----------------------------------------------+-------+------------------+
| 5  | Oil Temperature      | Transmission oil temperature                  | K     | 16-bit unsigned  |
+----+----------------------+-----------------------------------------------+-------+------------------+
| 6  | Transmission Status  | Bitmask indicating transmission status        |       | 8-bit bitmap     |
+----+----------------------+-----------------------------------------------+-------+------------------+
| 7  | Reserved             | Reserved for future use                       |       | 8-bit unsigned   |
+----+----------------------+-----------------------------------------------+-------+------------------+


**Gear Status (2-bit value)**

+-------+-------------+
| Value | Description |
+=======+=============+
| 0     | Forward     |
+-------+-------------+
| 1     | Neutral     |
+-------+-------------+
| 2     | Reverse     |
+-------+-------------+
| 3     | Reserved    |
+-------+-------------+

.. note::
   Aerial INS uses fields 1, 2, 4, 5, and 6 from this PGN. Field 6 is logged
   as a raw bitmap.

Logged topic: NMEA2000_TRANSMISSION


2.2.10 PGN 130816: Auxiliary Position (ANELLO Proprietary)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Auxiliary GPS / GNSS position information input

+---+--------+-------------------------------------------+------+----------------+
| # | Field  | Description                               | Unit | Type           |
+===+========+===========================================+======+================+
| 1 | lat    | Latitude                                  | deg  | 32-bit signed  |
+---+--------+-------------------------------------------+------+----------------+
| 2 | lon    | Longitude                                 | deg  | 32-bit signed  |
+---+--------+-------------------------------------------+------+----------------+
| 3 | alt    | Altitude above mean sea level             | m    | 32-bit signed  |
+---+--------+-------------------------------------------+------+----------------+
| 4 | hacc   | Horizontal accuracy / uncertainty         | m    | 32-bit signed  |
+---+--------+-------------------------------------------+------+----------------+
| 5 | vacc   | Vertical accuracy / uncertainty           | m    | 32-bit signed  |
+---+--------+-------------------------------------------+------+----------------+

.. note::
   Aerial INS uses fields 1 through 5 from this PGN. Latitude and longitude
   are encoded as ``1e-7 deg/count``. Altitude is encoded as ``1e-3 m/count``. 
   Horizontal accuracy and vertical accuracy are encoded as ``1e-7 m/count``.

Logged topic: NMEA2000_POS

2.2.11 PGN 130817: Auxiliary Attitude (ANELLO Proprietary)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Auxiliary roll, pitch, and heading information along with accuracy estimates

+---+----------+-------------------------------------------+------+----------------+
| # | Field    | Description                               | Unit | Type           |
+===+==========+===========================================+======+================+
| 1 | roll     | Roll angle                                | deg  | 32-bit signed  |
+---+----------+-------------------------------------------+------+----------------+
| 2 | pitch    | Pitch angle                               | deg  | 32-bit signed  |
+---+----------+-------------------------------------------+------+----------------+
| 3 | heading  | Heading / yaw                             | deg  | 32-bit signed  |
+---+----------+-------------------------------------------+------+----------------+
| 4 | roll_acc | Roll accuracy / uncertainty               | deg  | 32-bit signed  |
+---+----------+-------------------------------------------+------+----------------+
| 5 | pitch_acc| Pitch accuracy / uncertainty              | deg  | 32-bit signed  |
+---+----------+-------------------------------------------+------+----------------+
| 6 | head_acc | Heading accuracy / uncertainty            | deg  | 32-bit signed  |
+---+----------+-------------------------------------------+------+----------------+

.. note::
   Aerial INS uses fields 1 through 6 from this PGN. All fields are encoded
   as ``1e-7 deg/count``.

Logged topic: NMEA2000_RPH

3. Output Messages
-------------------------
3.1 NMEA 0183 Output Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To configure NMEA 0183 over a serial port, update the following configs (see
`Configure ANELLO Aerial INS <https://docs-a1.readthedocs.io/en/aerial_ins/getting_started_aerialins.html#configure-anello-aerial-ins>`__
for instructions on changing settings):

* ``NM0183_CFG`` = ``1`` (RS232-1) **or** ``2`` (RS232-2)
* ``NM0183_ODR_GGA`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NM0183_ODR_RMC`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NM0183_ODR_APIMU`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NM0183_ODR_APINS`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)


The default baud rate is ``57600`` for RS232-1 and ``921600`` for RS232-2.
Units shipped prior to 2/19/2026 have a default baud rate of ``57600`` on both ports.
To change the baud rate use ``SER_TEL1_BAUD`` for RS232-1 or ``SER_TEL2_BAUD`` for RS232-2.

To configure NMEA 0183 over UDP, update the following configs (see
`Configure ANELLO Aerial INS <https://docs-a1.readthedocs.io/en/aerial_ins/getting_started_aerialins.html#configure-anello-aerial-ins>`__
for instructions on changing settings):

* ``NMUDP_EN`` = ``1``
* ``NMUDP_ODR_GGA`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NMUDP_ODR_RMC`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NMUDP_ODR_APIMU`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)
* ``NMUDP_ODR_APINS`` = ``5`` (output data rate; e.g. ``5`` = 5 Hz, ``0`` is no output)

UDP output uses port ``19550`` and UDP input uses port ``19551``.
External UDP output only occurs when ``NMUDP_MC_IP0`` through ``NMUDP_MC_IP3``
define a valid multicast group. Setting ``NMUDP_EN`` to ``1`` alone does not
produce external UDP output.

See :ref:`nmea0183-over-udp-parameters` for how to set the multicast IP.

3.1.1. RMC: Recommended Minimum Navigation Information
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Message Format**::

    $--RMC,hhmmss.ss,A,ddmm.mmmmmm,a,dddmm.mmmmmm,a,x.x,x.x,xxxx,x.x,a*hh

+--------+-------------+--------------------------------------------------------------------------+
| Index  | Part        | Description                                                              |
+========+=============+==========================================================================+
| 1      | hhmmss.ss   | Time (UTC)                                                               |
+--------+-------------+--------------------------------------------------------------------------+
| 2      | A           | Status, A = solution initialized, V = Void, heading not initialized      |
+--------+-------------+--------------------------------------------------------------------------+
| 3      | ddmm.mmmmmm | Latitude                                                                 |
+--------+-------------+--------------------------------------------------------------------------+
| 4      | a           | N or S                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 5      | dddmm.mmmmmm| Longitude                                                                |
+--------+-------------+--------------------------------------------------------------------------+
| 6      | a           | E or W                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 7      | x.x         | Speed over ground, knots                                                 |
+--------+-------------+--------------------------------------------------------------------------+
| 8      | x.x         | INS Heading, degrees (range from -180 to 180)                            |
+--------+-------------+--------------------------------------------------------------------------+
| 9      | xxxx        | Date, ddmmyy                                                             |
+--------+-------------+--------------------------------------------------------------------------+
| 10     | x.x         | Magnetic Variation, degrees                                              |
+--------+-------------+--------------------------------------------------------------------------+
| 11     | a           | E or W                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 12     | hh          | Checksum                                                                 |
+--------+-------------+--------------------------------------------------------------------------+

.. note::
    Fields 2 and 8 differ from NMEA0183 standard for RMC output. Fields 10 and
    11 are left blank in current Aerial INS output.

3.1.2. GGA: Global Positioning System Fix Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Message Format**::

    $--GGA,hhmmss.ss,ddmm.mmmmmm,a,dddmm.mmmmmm,a,x,xx,x.x,x.x,M,x.x,M,x.x,xxxx*hh

+--------+-------------+--------------------------------------------------------------------------+
| Index  | Part        | Description                                                              |
+========+=============+==========================================================================+
| 1      | hhmmss.ss   | Time (UTC)                                                               |
+--------+-------------+--------------------------------------------------------------------------+
| 2      | ddmm.mmmmmm | Latitude                                                                 |
+--------+-------------+--------------------------------------------------------------------------+
| 3      | a           | N or S                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 4      | dddmm.mmmmmm| Longitude                                                                |
+--------+-------------+--------------------------------------------------------------------------+
| 5      | a           | E or W                                                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 6      | x           | GPS Quality Indicator (see table below)                                  |
+--------+-------------+--------------------------------------------------------------------------+
| 7      | xx          | Number of satellites in use (00-12)                                      |
+--------+-------------+--------------------------------------------------------------------------+
| 8      | x.x         | Horizontal Dilution of Precision (HDOP)                                  |
+--------+-------------+--------------------------------------------------------------------------+
| 9      | x.x         | Altitude (MSL)                                                           |
+--------+-------------+--------------------------------------------------------------------------+
| 10     | M           | Units of altitude (M=Meters)                                             |
+--------+-------------+--------------------------------------------------------------------------+
| 11     | x.x         | Geoidal separation                                                       |
+--------+-------------+--------------------------------------------------------------------------+
| 12     | M           | Units of geoidal separation (M=Meters)                                   |
+--------+-------------+--------------------------------------------------------------------------+
| 13     | x.x         | Age of differential data (seconds)                                       |
+--------+-------------+--------------------------------------------------------------------------+
| 14     | xxxx        | Differential Reference Station ID (0000-1023)                            |
+--------+-------------+--------------------------------------------------------------------------+
| 15     | hh          | Checksum                                                                 |
+--------+-------------+--------------------------------------------------------------------------+

**GPS Quality Indicator**

+-------+------------------------------------------------------------------+
| Value | Description                                                      |
+=======+==================================================================+
| 0     | Fix not available or invalid                                     |
+-------+------------------------------------------------------------------+
| 1     | GPS fix (no corrections)                                         |
+-------+------------------------------------------------------------------+
| 2     | Differential GPS fix                                             |
+-------+------------------------------------------------------------------+
| 4     | RTK Fixed                                                        |
+-------+------------------------------------------------------------------+
| 5     | RTK Float                                                        |
+-------+------------------------------------------------------------------+
| 6     | Dead reckoning mode (GPS is determined to be jammed or spoofed)  |
+-------+------------------------------------------------------------------+

3.1.3 IMU: Proprietary IMU Output
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Message Format**::

    $PAPIMU,Time,T_Sync,AX,AY,AZ,WX,WY,WZ,OG_WX,OG_WY,OG_WZ,MAG_X,MAG_Y,MAG_Z,TempC,Status_X,Status_Y,Status_Z*hh

+-------+----------+-------+--------------------------------------------------------------------------+
| Index | Field    | Units | Description                                                              |
+=======+==========+=======+==========================================================================+
| 1     | Time     | ms    | Time since power on                                                      |
+-------+----------+-------+--------------------------------------------------------------------------+
| 2     | T_Sync   | ms    | IMU synchronization timestamp                                            |
+-------+----------+-------+--------------------------------------------------------------------------+
| 3     | AX       | g     | X-Axis Acceleration                                                      |
+-------+----------+-------+--------------------------------------------------------------------------+
| 4     | AY       | g     | Y-Axis Acceleration                                                      |
+-------+----------+-------+--------------------------------------------------------------------------+
| 5     | AZ       | g     | Z-Axis Acceleration                                                      |
+-------+----------+-------+--------------------------------------------------------------------------+
| 6     | WX       | deg/s | X-Axis Angular Rate (MEMS)                                               |
+-------+----------+-------+--------------------------------------------------------------------------+
| 7     | WY       | deg/s | Y-Axis Angular Rate (MEMS)                                               |
+-------+----------+-------+--------------------------------------------------------------------------+
| 8     | WZ       | deg/s | Z-Axis Angular Rate (MEMS)                                               |
+-------+----------+-------+--------------------------------------------------------------------------+
| 9     | OG_WX    | deg/s | High Precision X-Axis Angular Rate (ANELLO Optical Gyro)                 |
+-------+----------+-------+--------------------------------------------------------------------------+
| 10    | OG_WY    | deg/s | High Precision Y-Axis Angular Rate (ANELLO Optical Gyro)                 |
+-------+----------+-------+--------------------------------------------------------------------------+
| 11    | OG_WZ    | deg/s | High Precision Z-Axis Angular Rate (ANELLO Optical Gyro)                 |
+-------+----------+-------+--------------------------------------------------------------------------+
| 12    | MAG_X    | Gauss | X-Axis Magnetic Field Measurement                                        |
+-------+----------+-------+--------------------------------------------------------------------------+
| 13    | MAG_Y    | Gauss | Y-Axis Magnetic Field Measurement                                        |
+-------+----------+-------+--------------------------------------------------------------------------+
| 14    | MAG_Z    | Gauss | Z-Axis Magnetic Field Measurement                                        |
+-------+----------+-------+--------------------------------------------------------------------------+
| 15    | TempC    | °C    | Temperature                                                              |
+-------+----------+-------+--------------------------------------------------------------------------+
| 16    | Status_X |       | Status bitfield for X-axis SiPhOG (see table below)                      |
+-------+----------+-------+--------------------------------------------------------------------------+
| 17    | Status_Y |       | Status bitfield for Y-axis SiPhOG (see table below)                      |
+-------+----------+-------+--------------------------------------------------------------------------+
| 18    | Status_Z |       | Status bitfield for Z-axis SiPhOG (see table below)                      |
+-------+----------+-------+--------------------------------------------------------------------------+

**APIMU Status Bits**

+-------+---------------------------------------------------------------+
| Bit   | Meaning                                                       |
+=======+===============================================================+
| 0     | Reserved in current Aerial INS output and always ``0``        |
+-------+---------------------------------------------------------------+
| 1     | Temperature uncontrolled                                      |
+-------+---------------------------------------------------------------+
| 2     | Over-current error                                            |
+-------+---------------------------------------------------------------+
| 3     | SiPhOG supply-voltage error                                   |
+-------+---------------------------------------------------------------+
| 4-7   | Unused and always ``0``                                       |
+-------+---------------------------------------------------------------+

3.1.4 INS: Proprietary Navigation Output
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Message Format**::

    $PAPINS,Time,PPS_Time,Status,Lat,Long,Height,VN,VE,VD,Roll,Pitch,Heading,Reserved*hh

.. list-table::
   :header-rows: 1
   :widths: 8 16 10 66

   * - Index
     - Field
     - Units
     - Description
   * - 1
     - Time
     - ms
     - Time since power on
   * - 2
     - PPS Time
     - ns
     - Currently always ``0.000``
   * - 3
     - Status
     -
     - Navigation status code (see table below)
   * - 4
     - Lat
     - deg
     - Latitude, ``+`` = North, ``-`` = South
   * - 5
     - Long
     - deg
     - Longitude, ``+`` = East, ``-`` = West
   * - 6
     - Height
     - m
     - Height above ellipsoid
   * - 7
     - VN
     - m/s
     - North velocity in NED frame
   * - 8
     - VE
     - m/s
     - East velocity in NED frame
   * - 9
     - VD
     - m/s
     - Down velocity in NED frame
   * - 10
     - Roll
     - deg
     - Roll angle about body X
   * - 11
     - Pitch
     - deg
     - Pitch angle about body Y
   * - 12
     - Heading
     - deg
     - Heading angle about body Z
   * - 13
     - Reserved
     -
     - Currently emitted empty

**APINS Status Values**

.. list-table::
   :header-rows: 1
   :widths: 10 90

   * - Value
     - Meaning
   * - 0
     - Attitude only using internal GNSS selection
   * - 1
     - Position and attitude using internal GNSS selection
   * - 2
     - Position, attitude, and heading using internal GNSS selection
   * - 8
     - Attitude only with GPS commanded off by the user
   * - 9
     - Position and attitude with GPS commanded off by the user
   * - 10
     - Position, attitude, and heading with GPS commanded off by the user
   * - 15
     - Position and attitude while external GNSS is selected
   * - 16
     - Position, attitude, and heading while external GNSS is selected
   * - 20
     - Dead reckoning after GNSS loss following prior GNSS fusion

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
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

High-speed update of vessel latitude/longitude position.

+---+-------------+----------------------+--------+----------------+
| # | Field       | Description          | Unit   | Type           |
+===+=============+======================+========+================+
| 1 | Latitude    | Position latitude    | deg    | 32-bit signed  |
+---+-------------+----------------------+--------+----------------+
| 2 | Longitude   | Position longitude   | deg    | 32-bit signed  |
+---+-------------+----------------------+--------+----------------+

.. note::
   Current Aerial INS output encodes latitude and longitude as
   ``1e-7 deg/count``.


3.2.2 PGN 129026: COG & SOG, Rapid Update
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Current Aerial INS output sets field 2 to ``0``.


3.2.3 PGN 129029: GNSS Position Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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
| 8   | Method                  | No fix, RTK, etc. (see table below)   |      | 4-bit lookup     |
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

.. note::
   Current Aerial INS output sets field 7 to ``0``, field 9 to ``0``,
   field 15 to ``1``, and field 16 to ``6``. Field 17 comes from the GNSS
   reference ID. Latitude and longitude are encoded as ``1e-16 deg/count``.
   Altitude is encoded as ``1e-6 m/count``.

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

.. note::
   Current Aerial INS output uses field 8 for the reported GNSS method.



3.2.4 PGN 127250: Vessel Heading
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Current Aerial INS output sets field 5 to ``0``.


3.2.5 PGN 127251: Rate of Turn
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Provides vessel rate of turn information.

+---+---------------+-----------------------------+--------+----------------+
| # | Field         | Description                 | Unit   | Type           |
+===+===============+=============================+========+================+
| 1 | SID           | Sequence Identifier         |        | 8-bit unsigned |
+---+---------------+-----------------------------+--------+----------------+
| 2 | Rate of Turn  | Positive = turn to starboard| rad/s  | 32-bit signed  |
+---+---------------+-----------------------------+--------+----------------+


3.2.6 PGN 127257: Attitude
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Current Aerial INS output encodes yaw, pitch, and roll as
   ``1e-4 rad/count``.


3.2.7 PGN 126992: System Time
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

.. note::
   Current Aerial INS output sets field 2 to ``0``.
