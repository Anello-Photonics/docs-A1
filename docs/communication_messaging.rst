Communication & Messaging
===========================

1  Interfacing
--------------------------

The communication interfaces currently supported for the ANELLO Maritime INS:
    1. Serial RS232
    2. Ethernet

1.1 Serial Communication Parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Default Baud Rate:
    - 57600

RS-232 Voltage Levels: 
    - +/- 7V

Data Format:
    - Data Bits: 8
    - Stop Bits: 1 
    - Parity: None

1.2 Port Definitions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For all interfaces, there are two main port types. 

1. RS232-1, The configuration and data port
2. RS232-2, Input port for speed aiding sensors


    +--------------------+------------------------------------------+---------------------------------------+
    | **Logical Port**   |  **Physical Port**                       |  **Functions**                        |
    +--------------------+------------------------------------------+---------------------------------------+
    | RS232-1            |                                          |                                       |
    |                    +------------------------------------------+                                       |
    |                    |                                          |                                       |
    +--------------------+------------------------------------------+---------------------------------------+
    | RS232-2            |                                          |                                       |
    |                    +------------------------------------------+                                       |
    |                    |                                          |                                       |
    +--------------------+------------------------------------------+---------------------------------------+
    | Ethernet           |                                          |                                       |
    +--------------------+------------------------------------------+---------------------------------------+

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

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


2  NMEA 0183 Data Output Messages
---------------------------------

The ANELLO Maritime INS supports standard NMEA 0183 input messages which allow the USV to send in external sensor information, e.g. for speed-aiding. ANELLO also has a set of proprietary messages, following the standard NMEA proprietary format with a prefix of “$P”, company code of “AP” (ANELLO Photonics), and the message code. 

 

The minimum sensor aiding for the ANELLO Maritime INS is velocity aiding via either a paddle wheel, doppler velocity log (DVL), or another source. 



2.1 RPM: Revolutions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


$--RPM,a\ :sub:`1` \,x\ :sub:`2` \,x.x\ :sub:`3` \,x.x\ :sub:`4` \,A\ :sub:`5` \*hh\ :sub:`6` |  

1) Source; S = Shaft, E = Engine 
2) Engine or shaft number 
3) Speed, Revolutions per minute 
4) Propeller pitch, % of maximum, "-" means astern  
5) Status, A means data is valid  
6) Checksum  

2.2 RSA: Rudder Sensor Angle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

$--RSA,x.x\ :sub:`1` \,A\ :sub:`2` \,x.x\ :sub:`3` \,A\ :sub:`4` \*hh\ :sub:`5` \  

1) Starboard (or single) rudder sensor, "-" means Turn To Port  
2) Status, A means data is valid 
3) Port rudder sensor 
4) Status, A means data is valid  
5) Checksum  

2.3 VHW: Water Speed & Heading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$--VHW,x.x\ :sub:`1` \,T\ :sub:`2` \,x.x\ :sub:`3` \,M\ :sub:`4` \,x.x\ :sub:`5` \,N\ :sub:`6` \,x.x\ :sub:`7` \,K\ :sub:`8` \*hh\ :sub:`9` \  

1) Degrees True 
2) T = True 
3) Degrees Magnetic 
4) M = Magnetic 
5) Knots (speed of vessel relative to the water) 
6) N = Knots 
7) Kilometers (speed of vessel relative to the water)  
8) K = Kilometres per hour 
9) Checksum  


2.4 VBW: Dual Ground/Water Speed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$--VBW,x.x\ :sub:`1` \,x.x\ :sub:`2` \,A\ :sub:`3` \,x.x\ :sub:`4` \,x.x\ :sub:`5` \,A\ :sub:`6` \*hh\ :sub:`7` \  

1) Longitudinal water speed, “-“ means astern 
2) Transverse water speed, “-“ means port 
3) Status, A = Data Valid 
4) Longitudinal ground speed, “-“ means astern 
5) Transverse ground speed, “-“ means port 
6) Status, A = Data Valid 
7) Checksum 

2.5 VWR: Relative Wind Speed & Angle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 
$--VWR,x.x\ :sub:`1` \,a\ :sub:`2` \,x.x\ :sub:`3` \,N\ :sub:`4` \,x.x\ :sub:`5` \,M\ :sub:`6` \,x.x\ :sub:`7` \,K\ :sub:`8` \*hh\ :sub:`9` \  

1) Wind direction magnitude in degrees  
2) Wind direction Left/Right of bow 
3) Speed 
4) N = Knots  
5) Speed 
6) M = Meters Per Second  
7) Speed 
8) K = Kilometers Per Hour  
9) Checksum 

2.6 $PAPGPSCTRL: GPS Control 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

$PAPGPSCTRL,x\ :sub:`1` \*hh\ :sub:`2` \  

1) GPS control, “1” = Use GPS (default), “0” = Ignore GPS 
2) Checksum   

3 ANELLO Custom Binary Sensor Input Message
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In addition to standard NMEA messages, the ANELLO Maritime INS supports a custom binary input message which can be used to populate available sensor information from an external GPS, a paddle wheel sensor, an external magnetometer, a wind speed and direction, and motor and rudder percentage information. This message is detailed below. 
 
**Serial communication protocol0**: RS-232 

**Baud rate**: 115200 (8 data bits, 1 stop bit, no parity, no hardware flow control) (other baud rates available upon request) 

**Transmission rate**: Up to 10 Hz (4 Hz default) 

**Endianness**: All fields are big endian 



