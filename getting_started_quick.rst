Quick Getting Started Guide
==================================

1. A-1 Hardware Connections
---------------------------------
The Anello A-1 EVK includes the eight items highlighted below:

.. figure:: media/evk_contents.png
   :align: center
   
   Figure 1: Anello A-1 EVK Contents

    +---+------------------------------------------------+
    | 1 | A-1 Inertial Navigation System                 +
    +---+------------------------------------------------+
    | 2 | Two Dual-Band Multi-Constellation GNSS Antennae|
    +---+------------------------------------------------+
    | 3 | A-1 System Power Cable                         |
    +---+------------------------------------------------+
    | 4 | 110-240VAC Wall-Power Adaptor Plug             |
    +---+------------------------------------------------+
    | 5 | International Wall Power Plug Inserts          |
    +---+------------------------------------------------+
    | 6 | In-Vehicle Power Adaptor                       |
    +---+------------------------------------------------+
    | 7 | USB-C Cable                                    |
    +---+------------------------------------------------+
    | 8 | Ethernet Cable                                 |
    +---+------------------------------------------------+

Connect the hardware as follows: 

.. figure:: media/evk_wiring.png
   :scale: 50 %
   :align: center

   Figure 2: Anello A-1 Connection Diagram

1. Connect A-1 power connection using either the wall-power adaptor or the in-vehicle adaptor (green/yellow).  
2. Connect A-1 to computing system using USB-C (red) for configuration. (If A-1 is already configured, Ethernet interface (black) is recommended for data collection.)
3. Connect primary GNSS antenna to GPS1 on the back of the A-1 (blue). An additional antenna to GPS2 is optional.


4. A-1 Configurations
---------------------------------
2.1 Install Anello Python Program
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Confirm that Python is installed and the version is at least 3.6:

.. code-block:: python
    
    >python -V

Clone the GitHub repository:

.. code-block:: python
    
    >cd <local directoy to store Anello Python Program>
    >git clone https://github.com/Anello-Photonics/user_tool.git

Install dependencies using pip:

.. code-block:: python
    
    >cd user_tools
    >pip install -r requirements.txt

2.2 Run the Tool 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python
    
    >cd board_tools
    >python user_program.py

2.3 Connect to the A-1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Use the arrows to select *Connect* and press enter. Select *COM* and then *Auto*. The unit will
be auto detected via Serial over USB-C. 

Select the COM ports. The Anello A-1 uses two logical ports: 

    +-------------------------+-----------------------------------+
    | **Logical Port**        |  **Physical Port** (Serial/USB-C) |
    +-------------------------+-----------------------------------+
    |  Data Port              | lowest port number e.g., COM7     |
    +-------------------------+-----------------------------------+
    |  Configuration  Port    | highest port number e.g., COM10   |
    +-------------------------+-----------------------------------+

2.4 Adjust Unit Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Select *Configure* from the main menu. To change a configuration from the default, select *Edit*, 
then the configuration to change, then select the new value.

The A-1 Ethernet (UDP) interface is recommended for in-vehicle data collection. To connect by UDP over Ethernet: 

1. Connect to the A-1 over COM (section 2.3).
2. In *Configure*, set the A-1 IP address statically or automatically using DHCP (default)
3. Set the IP address of where you want the A-1 to send data, i.e., the receiving computer's IP
4. Set the Data Port and User Messaging Port numbers
5. Connect to the A-1 via UDP instead of USB. Use the same A-1 IP, configuration port and data port as in steps 2 & 3.

**Congratulations!!!**
You have completed the initial setup of the Anello A-1.


3. A-1 Data Collection
---------------------------------
3.2 Log a Data File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Select *Log* in the main menu, then *Start*. Use either the default name or enter a custom name.
To end the log, select *Log* and then *Stop*.

The log files are saved in the "logs" directory within user_tools, grouped by month and then day.

To export a log file to CSV, Select *Log* in the main menu, then *Export*, then choose the log file.
Three CSV files will be saved in the "exports" directory, under the name of the original log file:

-   imu.csv : raw IMU data such as acceleration and angular rates (APIMU messages)
-   gps.csv : GNSS data (APGPS messages)
-   ins.csv : primary inertial navigation solution data (APINS messages)

If the A-1 antenna was collecting GNSS data during logging, the exported CSVs can be visualized at `Kepler <https://kepler.gl/demo>`_, an online tool for geo-spatial data analysis. 

3.3 Monitor Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Monitoring mode opens a display to watch the data of the INS solution in real-time.
It also supports toggling the logging and GNSS connection with the LOG and GPS buttons

To start monitoring, select *Monitor* in the main menu. This will launch a separate window.

.. figure:: media/monitoring.png
   :scale: 50 %
   :align: center

   Live Output Monitoring

3.4 Connect to NTRIP Caster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Connecting to an NTRIP caster will improve the accuracy of GNSS positioning using RTK corrections.
For firmware versions 0.4.3 and earlier, NTRIP requires the A-1 to be connected by UDP.

From the main menu, select *NTRIP* and then *Start*. Enter the NTRIP caster details as prompted. 
The *System Status* will show the NTRIP connection status.

4 A-1 Vehicle Installation
----------------------------
4.1 Set Vehicle Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
From the main menu, select *Vehicle Configurations* to set the positions as prompted.

4.2 Install the A-1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The mounting location of the A-1 is flexible and can be configured for various installation positions.  
To minimize configuration, mount the unit near the venter of the vehicle’s rear axle, with the X-Axis facing 
the direction of travel.

.. figure:: media/a1_install_location.png
   :scale: 50 %
   :align: center

   Default A-1 Installation Location

The GNSS antennae should be magnetically mounted on the roof of the vehicle.

**Congratulations!!!**
You are now ready to collect data!  Note that the system requires exceeding 2m/s velocity to enter full INS mode, 
and the performance will generally improve after the first 5 minutes of driving.