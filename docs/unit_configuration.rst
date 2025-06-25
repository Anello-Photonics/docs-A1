Unit Configurations
=======================

The easiest way to configure an ANELLO unit is using the `ANELLO Python Program <https://docs-a1.readthedocs.io/en/latest/python_tool.html#unit-configurations>`_, 
which saves all changes to non-volatile flash memory. 

Alternatively, the unit can be configured using the `APCFG message <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html#apcfg-messages>`_.

Unit Configuration Settings
-----------------------------------
The available parameters and values to configure are described in the table below:


IMU / IMU+:
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
  | Output Message Format  | mfm        | Format of the output messages. 1: ASCII, 4: RTCM (default)                                                  |
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
The ANELLO IMU+ supports ODR up to 200 Hz. RTCM message format is recommended for best timing.

.. note:: Decreasing the baud rate will affect the maximum output data rate. It is recommended to keep the default baud rate (230400 for IMU+) enable highest ODR.

Digital Filters
~~~~~~~~~~~~~~~~~~~
Fixed-point digital filters are implemented in the firmware and operate on the raw sensors readings (counts) prior to conversion to scaled 
sensor readings (in [g] and [°/s]). Cutoff frequencies can be selected by the user using the APCFG command for the accelerometers (lpa), 
MEMS angular-rate sensors (lpw), and optical gyroscopes (lpo).

Any integer value between zero and 90% of Nyquist frequency (0.5*ODR) can be selected. A zero value disables filtering and any value above 90% Nyquist is limited.


AZUPT for ANELLO AHRS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Zero velocity update (ZUPT) is used to tell the IMU AHRS system that it is stationary. The user should only command this mode when the user can confirm that the system is stationary and turn off the mode before motion starts. While ZUPT is on, heading is locked, roll and pitch are estimated with accelerometer values, and angular rate biases are estimated. While ZUPT is off, the angular rates have the biases subtracted before being input into the filter 