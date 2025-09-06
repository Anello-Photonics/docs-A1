Unit Configurations
=======================

The easiest way to configure an ANELLO unit is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/latest/python_tool.html#unit-configurations>`_, 
which saves all changes to non-volatile flash memory. 

Alternatively, the unit can be configured using the `APCFG message <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html#apcfg-messages>`_.

Unit Configuration Settings
-----------------------------------
The available parameters and values to configure are described in the table below:

GNSS / INS and ANELLO EVK:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Configuration          | APCFG Code | Value/Description                                                                                           |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Orientation            | orn        | Coordinate axes for mounting position, default +X+Y+Z (see below)                                           |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Alignment Angles       | aln        | Alignment angles of unit in degrees, in +X+Y+Z (roll, pitch, yaw) order. Default +0.0+0.0+0.0               |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Output Data Rate       | odr        | Output rate of APIMU message: 20, 50, 100, or 200 Hz. Requires reset.                                       |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable GPS 1           | gps1       | Use primary antenna (ANT1): 'on', 'off'                                                                     |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable GPS 2           | gps2       | Use secondary antenna (ANT2): 'on', 'off'                                                                   |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Odometer Units         | odo        | Speed unit of the odometer message: 'mps', 'mph', 'kph', 'fps'                                              |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable FOG             | fog        | Enable optical gyro data to be used in algorithm: 'on', 'off'                                               |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | DHCP (auto-assign IP)  | dhcp       | DHCP (let router assign EVK IP for UDP messaging): 'on', 'off'                                              |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | EVK UDP Port           | lip        | EVK's IP address, applies only if DHCP off: aaa.bbb.ccc.ddd                                                 |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Computer UDP Port      | rip        | IP address of computer for UDP communication: aaa.bbb.ccc.eee                                               |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | UDP Data Port          | rport1     | Computer's UDP port for data output (works like data serial port): integer from 1 to 65535                  |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | UDP Configuration Port | rport2     | Computer's UDP port for config messaging (works like data serial port): integer from 1 to 65535             |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | UDP Odometer Port      | rport3     | Computer's UDP port for odometer messaging: integer from 1 to 65535                                         |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | APCFG Output           | min        | Minutes between output of APCFG configuration values over the data port (0 disables output)                 |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Accel Cutoff Freq      | lpa        | Low-pass filter cutoff frequency [Hz] for the MEMS accelerometer (0 disables filter)                        |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | MEMS Gyro Cutoff Freq  | lpw        | Low-pass filter cutoff frequency [Hz] for the MEMS angular rate sensor (0 disables filter)                  |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | FOG Cutoff Freq        | lpo        | Low-pass filter cutoff frequency [Hz] for the optical gyro (0 disables filter)                              |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | NTRIP Input Channel    | ntrip      | Input channel for NTRIP data. 0: off (default), 1: Serial, 2: UDP                                           |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Sync Pulse Enable      | sync       | Enables the external synchronization pulse input: 'on', 'off'                                               |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Output Message Format  | mfm        | Format of the output messages. 1: ASCII, 4: RTCM (default)                                                  |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable Serial Output   | uart       | Enable output over serial interface: 'on', 'off'                                                            |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable Ethernet Output | eth        | Enable output over ethernet interface: 'on', 'off'                                                          |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | NMEA Output Messages   | nmea       | Configures NMEA messages to be output on the config port. Bit 0 = GGA, Bit 1 = GSA, Bit 2 = RMC             |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Baud Rate              | bau        | Serial communication baud rate in bits per second. Requires reset.                                          |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | NHC                    | nhc        | Non-holonomic constraint. 0: Default Vehicle, 2: Heavy Vibration Vehicle, 7: NHC off (not recommended)      |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+

.. note:: The UDP ports are the numbers on the connected computer only. The EVK uses UDP port 1 for data, 2 for configuration, and 3 for odometer.

IMU / IMU+ and ANELLO X3:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Configuration          | APCFG Code | Value/Description                                                                                           |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Output Data Rate       | odr        | Output rate of APIMU message: 20, 50, 100, or 200 Hz. Requires reset.                                       |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Accel Cutoff Freq      | lpa        | Low-pass filter cutoff frequency [Hz] for the MEMS accelerometer (0 disables filter)                        |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | MEMS Gyro Cutoff Freq  | lpw        | Low-pass filter cutoff frequency [Hz] for the MEMS angular rate sensor (0 disables filter)                  |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | FOG Cutoff Freq        | lpo        | Low-pass filter cutoff frequency [Hz] for the optical gyro (0 disables filter)                              |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Baud Rate              | bau        | Serial communication baud rate in bits per second. Requires reset.                                          |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Sync Pulse Enable      | sync       | Enables the external synchronization pulse input: 'on', 'off'                                               |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Output Message Format  | mfm        | Format of the output messages. 0: Binary (X3 only), 1: ASCII, 4: RTCM Binary (GNSS INS, IMU+, and EVK only) |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+

Additional IMU+ ANELLO AHRS Commands:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Configuration          | APCFG Code | Value/Description                                                                                           |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable AHRS            | ahrs       | Enables or disables the AHRS filter output and calculations. This command is flash only.                    |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Enable ZUPT            | azupt      | Configures the ZUPT mode for the AHRS filter. 0 is off, 1 is heading lock. This command is RAM only.        |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+
  | Heading Update         | ahdg       | Allows the user to load in a custom heading to the AHRS filter. Input format is degrees * 1000 to give 3    |
  |                        |            | decimals of precision. Ex: 180.123 would be loaded as  180123. This command is RAM only.                    |
  +------------------------+------------+-------------------------------------------------------------------------------------------------------------+

.. note:: Some configurations require a system reset after changing, such as the ODR and baud rate. This can be done by selecting "Reset" in the user_program.py main menu, or sending the reset command over the Configuration port: #APRST,0*58 



Output Data Rate (ODR)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The GNSS INS has output data rate constraints when outputting data over RS-232. In RTCM or binary messaging mode, 
maximum ODR is 100 Hz. In ASCII mode, maximum ODR is 50 Hz.
All other ANELLO units support ODR up to 200 Hz. RTCM message format is recommended for best timing.

.. note:: Decreasing the baud rate will affect the maximum output data rate. It is recommended to keep the default baud rate (921600 for EVK; 230400 for GNSS INS and IMU) enable highest ODR.

Digital Filters
~~~~~~~~~~~~~~~~~~~
Fixed-point digital filters are implemented in the firmware and operate on the raw sensors readings (counts) prior to conversion to scaled 
sensor readings (in [g] and [°/s]). Cutoff frequencies can be selected by the user using the APCFG command for the accelerometers (lpa), 
MEMS angular-rate sensors (lpw), and optical gyroscopes (lpo).

Any integer value between zero and 90% of Nyquist frequency (0.5*ODR) can be selected. A zero value disables filtering and any value above 90% Nyquist is limited.

Unit Installation Orientation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Orientation describes the mounting orientation of the ANELLO Unit on the vehicle. 
This configuration is only used in the ANELLO algorithm and does not affect IMU data output.

The following 8 right hand rule frames are possible:

    1. +X+Y+Z  Default; Unit mounted upright with X pointing in vehicle forward
    2. +Y-X+Z  Unit mounted upright with X pointing in vehicle left
    3. -Y+X+Z  Unit mounted upright with X pointing in vehicle right
    4. -X-Y+Z  Unit mounted upright with X pointing in vehicle back
    5. +X-Y-Z  Unit mounted upside down with X pointing in vehicle forward
    6. +Y+X-Z  Unit mounted upside down with X pointing in vehicle right
    7. -Y-X-Z  Unit mounted upside down with X pointing in vehicle left
    8. -X+Y-Z  Unit mounted upside down with X pointing in vehicle back

ANELLO Unit Installation Misalignment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Misalignment in the unit installation will degrade performance, particularly in GNSS-denied periods.
ANELLO recommends the following procedure for calibrating roll and pitch installation misalignment angles.
Please ensure the Alignment Angles configuration is set to (0,0,0) before starting the procedure.

1. Park the vehicle in any location and take note of the roll and pitch reported by the ANELLO unit.
2. Park the vehicle in the same location but rotated 180 degrees, and take note of the roll and pitch reported by the ANELLO unit.
3. Ensure that there is no difference in side or front/rear load on the vehicle which may affect the roll or pitch between the two tests.
4. Calculate roll misalignment angle = (roll_1 + roll_2) / 2
5. Calculate pitch misalignment angle = (pitch_1 + pitch_2) / 2
6. Use the calculated roll and pitch misalignment angles, and any known heading misalignment angles, to set the Alignment Angles configuration using the ANELLO Python tool or the APCFG command with aln code.

Example:
roll_1 = 5.0, pitch_1 = -10.0
roll_2 = 1.0, pitch_2 = 0.0

Roll misalignment angle = (roll_1 + roll_2) / 2 = 3.0
Pitch misalignment angle = (pitch_1 + pitch_2) / 2 = -5.0

Alignment Angle (roll, pitch, yaw) = +3.0, -5.0, 0.0

After setting the configuration and restarting the unit, the unit should now show roll and pitch equivalent to the slope of the ground when parked in the same spot.
For reference, the slope of the ground using the example above can be calculated by:

Roll slope = (roll_1 - roll_2) / 2 = 2.0
Pitch slope = (pitch_1 - pitch_2) / 2 = -5.0

AZUPT for ANELLO AHRS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Zero velocity update (ZUPT) is used to tell the IMU AHRS system that it is stationary. The user should only command this mode when the user can confirm that the system is stationary and turn off the mode before motion starts. While ZUPT is on, heading is locked, roll and pitch are estimated with accelerometer values, and angular rate biases are estimated. While ZUPT is off, the angular rates have the biases subtracted before being input into the filter 