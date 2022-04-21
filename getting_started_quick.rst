==================================
Getting Started Guide
==================================
Thank you for choosing the Anello A-1! The following guide will get you started with hardware configuration and data collection.
Please contact support@anellophotonics.com with any questions.  

1   A-1 Hardware Connections
---------------------------------
The Anello A-1 Evaluation Kit (EVK) includes the following items:

    +---+------------------------------------------------+
    | 1 | Anello A-1 EVK                                 |
    +---+------------------------------------------------+
    | 2 | Two Dual-Band Multi-Constellation GNSS Antennae|
    +---+------------------------------------------------+
    | 3 | Power Cable                                    |
    +---+------------------------------------------------+
    | 4 | 110-240V AC Wall-Power Adaptor                 |
    +---+------------------------------------------------+
    | 5 | International Wall-Power Plug Inserts          |
    +---+------------------------------------------------+
    | 6 | In-Vehicle Power Adaptor                       |
    +---+------------------------------------------------+
    | 7 | USB Cable                                      |
    +---+------------------------------------------------+
    | 8 | Ethernet Cable                                 |
    +---+------------------------------------------------+

.. image:: media/evk_contents.png
   :scale: 100 %
   :align: center
|

Connect the hardware as follows: 

1. Connect A-1 to power using either the wall-power adaptor or the in-vehicle adaptor (red).
2. Connect A-1 to computing system using USB (blue) for configuration. (If A-1 is already configured, Ethernet interface (green) is recommended for data collection.)
3. Connect GNSS antenna to ANT1 on the back of  A-1 (black). An additional antenna (ANT2) is optional.

.. image:: media/evk_wiring_2.png
   :scale: 45 %
   :align: center
|
2   A-1 Configurations
---------------------------------
2.1 Install Anello Python Program
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
If you do not have Python installed, download here: `<https://www.python.org/downloads/>`_

Confirm that Python is installed and the version is at least 3.6:

.. code-block:: python
    
    >python -V

If you do not have a Git client installed, download here: `<https://git-scm.com/download>`_ 

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

You will see *System Status* at the top, showing the Connection, NTRIP, and Logging status.

2.3 Connect to the A-1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Use the arrow keys to select *Connect* and press enter. Select *COM* then *Auto* to auto-detect the unit. 

You should now see the *System Status* updated with the Device and Connection information.

Note: If four COM ports do not show in the manual connection mode or Windows device manager, 
you may need to install the FTDI drivers from https://ftdichip.com/drivers/d2xx-drivers/

2.4 A-1 Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Select *User Configuration* from the main menu to see default configurations. To change any configurations, 
select *Edit*, then the configuration to change, then select the new value.

2.5 Connect to NTRIP Caster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Connecting to an NTRIP caster will improve the accuracy of GNSS positioning using RTK corrections.

From the main menu, select *NTRIP* and then *Start*. Enter the NTRIP caster details as prompted. 
The *System Status* will show the NTRIP connection status.

|
**Congratulations!!!**
You have completed the initial setup of the Anello A-1.

|
3   A-1 Data Collection
---------------------------------
3.1 Monitor Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For a real-time display of the INS solution, select *Monitor* in the main menu.
To toggle the logging or GNSS connection, click the LOG or GPS button.


3.2 Log a Data File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In the main menu, select *Log*, then *Start*. Use the default filename or enter a custom name. 
The *System Status* will be updated with the logging information.

To end the log, select *Log* then *Stop*. Log files are saved in the "logs" directory in user_tools, 
grouped by month and day.

To export a log file to CSV, Select *Log* in the main menu, then *Export*, then choose the log file.
Three CSV files (imu.csv, gps.csv, and ins.csv) will be saved in the "exports" directory, under the name of the original log file.

Data can be visualized by importing ins.csv into `Kepler <https://kepler.gl/demo>`_

|
4   A-1 Vehicle Installation
----------------------------
4.1 Connect via Ethernet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The A-1 Ethernet (UDP) interface is recommended for in-vehicle data collection. To connect via UDP: 

1. If you haven't already, connect to the A-1 over COM (see Section 2.3).
2. Connect the A-1 to your computer using Ethernet (see Section 1)
3. In main menu, select *User Configuration*, then:
   
   a. Set the A-1 IP address statically or automatically using DHCP (default).
   b. Set receiving computer's IP.
   c. Set the Data Port and User Messaging Port.

4. In main menu, select *Connect* and choose *UDP*, then *Manual*, then:
   
   a. Enter the A-1 IP, Configuration port and data port from step 3.
   b. If a Windows Security Alert pops up, click "Allow Access" to enable UDP communication.
|
4.2 Install the A-1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The A-1 can be configured for various installation positions. To minimize configuration steps, 
mount the unit near the center of the vehicle’s rear axle, with the X-Axis facing the direction of travel.

.. image:: media/a1_install_location.png
   :scale: 25 %
   :align: center
|
The GNSS antennae can be magnetically mounted on the roof of the vehicle.

4.3 Set Vehicle Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
From the main menu, select *Vehicle Configurations* and set the positions as prompted.

|
**Congratulations!!!**
You have completed the A-1 setup! To collect data, please refer to Section 3. 
Note that the A-1 performance will improve after several minutes of driving.