==================================
Maritime INS Getting Started Guide
==================================

Thank you for choosing the ANELLO Maritime INS! This step-by-step guide will get you started with connection, configuration, and data collection.  
Please contact support@anellophotonics.com with any questions.  

1. Hardware Connections
---------------------------------

The ANELLO Maritime INS unit is pictured below. It features a 20-pin circular connector and two female SMA GNSS connectors.

.. image:: media/ANELLO_Maritime_INS.png
   :width: 25%
   :align: center

An SCD drawing of the Maritime INS and a schematic of the accessory kit breakout cable can be found in  
`Mechanicals <https://docs-a1.readthedocs.io/en/maritime_ins/mechanicals.html>`__.


2. Software Interfaces
---------------------------------

Connect with QGroundControl:

1. Install `QGroundControl <https://qgroundcontrol.com/>`_ on your computer.

2. Set the Ethernet IP address of the host computer to **192.168.0.2** and subnet mask to **255.255.255.0**.

.. image:: media/ethernet_settings.png
   :width: 50%
   :align: center

3. Open QGroundControl.

.. image:: media/QGroundControl-Disconnected.png
   :width: 60%
   :align: center

4. Set up the Ethernet connection in QGroundControl (only needs to be done once):

   a. Click the **Q** button (top right) → Application Settings → Comm Links → ETH  
   b. Type: UDP  
   c. Port: 14550

5. Connect the Maritime INS to the computer using Ethernet.

6. Once connected, the status in the top left of QGroundControl changes from **Disconnected** to **Not Ready**.

.. image:: media/QGroundControl-NotReady.png
   :width: 60%
   :align: center


3. Vehicle Installation
----------------------------

The ANELLO Maritime INS can be configured for various installation positions as long as parameters are set as detailed in the next section.  
An external speed-aiding sensor is highly recommended to maintain accuracy in GPS-denied conditions. Calibration procedures for common sensors are detailed in  
`Sensor Calibrations <https://docs-a1.readthedocs.io/en/maritime_ins/sensor_calibrations.html>`__.

It is recommended that the Maritime INS be installed with the **X axis facing forward** and as close to the centerline as possible.  
If this is not possible, configure **SENS_BOARD_ROT** and **EKF2_IMU_POS** offsets accordingly.

Below is the recommended installation configuration, with the longest possible antenna baseline (distance between antennae) with a minimum of 1m baseline to ensure optimal dual antenna heading accuracy.

Ensure that antennae are mounted on a ground plane of at least 10 cm x 10 cm and with no obstructions to open sky view.

.. image:: media/maritime_ins_installation.drawio.png
   :width: 60%
   :align: center


4. Configure ANELLO Maritime INS
---------------------------------

The lever arms of the installation must be measured and configured as parameters to ensure accuracy.  
The coordinate system follows the right-hand rule: **X = forward**, **Y = right**, **Z = down**.  
The INS center is the center of the Maritime INS unit.

Distances are measured in meters from the IMU center to the respective antenna phase center.

+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| Parameter           | Units | Default | Description                                                                       |
+=====================+=======+=========+===================================================================================+
| **GPS_SEP_BASE_X**  | m     | 0       | X offset from INS center to Base antenna (ANT1).                                  |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **GPS_SEP_BASE_Y**  | m     | 0       | Y offset from INS center to Base antenna (ANT1).                                  |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **GPS_SEP_BASE_Z**  | m     | 0       | Z offset from INS center to Base antenna (ANT1).                                  |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **GPS_SEP_ROVER_X** | m     | 0       | X offset from INS center to Rover antenna (ANT2).                                 |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **GPS_SEP_ROVER_Y** | m     | 0       | Y offset from INS center to Rover antenna (ANT2).                                 |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **GPS_SEP_ROVER_Z** | m     | 0       | Z offset from INS center to Rover antenna (ANT2).                                 |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **EKF2_GPS_YAW_OFF**| deg   | 0       | Yaw offset to align antenna heading with vessel heading;                          |
|                     |       |         | typically set to align coordinate frames.                                         |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **EKF2_IMU_POS_X**  | m     | 0       | X offset from center of boat to INS center.                                       |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **EKF2_IMU_POS_Y**  | m     | 0       | Y offset from center of boat to INS center.                                       |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **EKF2_IMU_POS_Z**  | m     | 0       | Z offset from center of boat to INS center.                                       |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+
| **SENS_BOARD_ROT**  | enum  | 0       | INS mounting orientation. Set this if unit is not mounted with X-forward.         |
|                     |       |         |                                                                                   |
|                     |       |         | *Common values:*                                                                  |
|                     |       |         |   - **0**: Unit mounted upright with X pointing in vehicle forward                |
|                     |       |         |   - **2**: Unit mounted upright with X pointing in vehicle left                   |
|                     |       |         |   - **4**: Unit mounted upright with X pointing in vehicle back                   |
|                     |       |         |   - **6**: Unit mounted upright with X pointing in vehicle right                  |
|                     |       |         |   - **12**: Unit mounted upside down with X pointing in vehicle forward           |
|                     |       |         |   - **14**: Unit mounted upside down with X pointing in vehicle right             |
|                     |       |         |   - **16**: Unit mounted upside down with X pointing in vehicle back              |
|                     |       |         |   - **18**: Unit mounted upside down with X pointing in vehicle left              |
|                     |       |         |                                                                                   |
|                     |       |         | Will be presented as a drop-down menu in QGroundControl.                          |
+---------------------+-------+---------+-----------------------------------------------------------------------------------+

To change parameters using QGroundControl: **Q > Vehicle Setup > Parameters**

.. image:: media/QGC_parameters.png
   :width: 60%
   :align: center


5. Data Collection & Visualization
------------------------------------

After installation and configuration, the unit is ready for data collection.  
Data is logged automatically once power is applied to the Maritime INS. No manual steps are required to start logging.

* Start a new log by cycling power to the unit.  
* Download logs in QGroundControl under **Q > Analyze Tools > Log Download**.  
* Use a plotting tool such as PlotJuggler for visualization. Contact ANELLO for assistance with post-processing, including GPS-denied simulations.

.. image:: media/QGC_logs.png
   :width: 60%
   :align: center



Some key topics in the log files are:


+-------------------------+-----------+----------------------------------------------------------------------------------------------------+
| Topic                   | instances | Description                                                                                        |
+=========================+===========+====================================================================================================+
| vehicle_global_position | 1         | Full INS solution containing latitude longitude coordinates                                        |
+-------------------------+-----------+----------------------------------------------------------------------------------------------------+
| senor_gps               | 2         | GNSS only solution from each receiver                                                              |
+-------------------------+-----------+----------------------------------------------------------------------------------------------------+
| senor_gps_heading       | 1         | GNSS Dual heading and baseline data                                                                |
+-------------------------+-----------+----------------------------------------------------------------------------------------------------+

6. Water Testing Procedure
-------------------------------

For best GPS-denied navigation results, ANELLO recommends the following initialization procedure after each startup:

1. Ensure the unit is powered off while launching the vehicle into the water.

2. While the USV is stationary in water, power on the unit. A good GPS signal is required for position initialization.

3. For best performance, first perform a short square mission with 30–50 m edges to give the system visibility into currents before GPS is lost.

4. Perform your mission. Best performance in GPS-denied conditions is achieved with calibrated speed aiding and at speeds above 2 knots.

*Maritime INS User Manual 93001501 v1.0.0*
