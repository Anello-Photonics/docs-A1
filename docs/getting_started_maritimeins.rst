==================================
Maritime INS Getting Started Guide
==================================

Thank you for choosing the ANELLO Maritime INS! This step-by-step guide will get you started with connection, configuration and data collection.
Please contact support@anellophotonics.com with any questions.  

1. Hardware Connections
---------------------------------

The ANELLO Maritime INS unit is pictured below. It features a 20 pin ... connector and two female SMA GNSS connectors

.. image:: media/ANELLO_Maritime_INS.png
   :width: 20 %
   :align: center


2. Software Interfaces
---------------------------------

Connect with QGroundControl:

Install `QGroundControl <https://qgroundcontrol.com/>`_ software onto your laptop or computer 

Connect the Maritime INS to computer using either the RS232 serial to USB or ethernet

Change ethernet IP address on host computer to "192.168.0.2"

Open QGroundControl. 


.. image:: media/QGroundControl-Disconnected.png
   :width: 60 %
   :align: center

Once connected, the text on the top left will change from “Disconnected” to “Not Ready” 

.. image:: media/QGroundControl-NotReady.png
   :width: 60 %
   :align: center



3. Vehicle Installation
----------------------------

The ANELLO Maritime INS can be configured for various installation positions. The vector from ANT1 to ANT2 should be parallel to vehicle forward; i.e., both antennae may be offset from the vehicle centerline, as long as it’s by the same amount. 


4. Configure ANELLO Maritime INS
---------------------------------

Orientation: If the box is facing forwards, the below should each be 0 (default). Otherwise, these values should be the angle offset in degrees. For example, if box is facing backwards, both X and Y should be set to 180. 

	SENS_BOARD_X_OFF 

	SENS_BOARD_Y_OFF 

	SENS_BOARD_X_OFF 

The lever arm to ANT1, with the center of the box as the origin and using forward (X), right (Y), down (Z) frame, should be entered in meters: 

	GPS_SEP_ANT_X 

	GPS_SEP_ANT_Y 

	GPS_SEP_ANT_Z 

If the antennae are aligned in any other orientation other than ANT1 in back and ANT2 in front, the GPS_YAW_OFF must be updated to account for the offset.  

For example, if ANT1 is on the left and ANT2 is on the right, GPS_YAW_OFF should be 90.  


6. Data Collection & Visualization
------------------------------------

After installing the box and configuring the units, you are ready for data collection. Data from the Maritime INS is logged automatically once power is applied to the box. There is no manual intervention needed to start a log. A couple notes: 

A new log can be started simply by cycling power to the ANELLO payload. 

Logs must be started in good GPS conditions, as GPS is currently used for global position initialization. 


7. Water Testing Procedure
-------------------------------

For best GPS-denied navigation results, ANELLO recommends the following initialization procedure after each startup: 

	1. ANELLO payload should be off while USV is launched into water. 

	2. While the USV is stationary in water with GPS signal, power on ANELLO payload. 

		a. Good GPS signal is currently required for initialization. 

		b. If you don’t see RMC output from the ANELLO unit, the system is not initialized yet. In good GPS conditions this typically takes less than 30 seconds. 

	3. Once you see data from ANELLO unit, the USV may start driving. Perform a short square mission with 30-50 meter edges to gain visibility into currents before GPS is lost. 

		a. This gives the system visibility into the sea currents and winds 

	4. After the square, you can perform your desired mission. Best GPS-denied performance is seen at speeds higher than 2 knots. 

		a. It is best to avoid driving backwards while GPS-denied for more than 10s at a time as paddle wheel behavior tends to be erratic during backwards driving. 

		b. To ensure ANELLO will be able to view the data in the log, it is recommended to keep the ANELLO system on only up to 5 hours at a time. (The ANELLO system will continue functioning and outputting messages beyond this time, but data will not be logged after this time.) 

	5. After completing mission, logs can be downloaded 


*Maritime INS User Manual 93001501 v1.0.0*