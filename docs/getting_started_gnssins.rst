==================================
GNSS INS Getting Started Guide
==================================
Thank you for choosing the ANELLO GNSS INS! This guide will get you started with connection, configuration and data collection.
Please contact support@anellophotonics.com with any questions.  

1   Hardware Connections
---------------------------------
The ANELLO GNSS INS unit is pictured below. It features a 20 pin automotive-grade Molex MX150 connector and two FAKRA SMB GNSS connectors.

.. image:: media/ANELLO_GNSS_INS.png
   :width: 50 %
   :align: center

If you purchased the GNSS INS Evaluation Kit, you will also receive the items pictured below. See `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html>`__ for schematic of the breakout cable.

.. image:: media/GNSS_INS_EvalKit.png
   :width: 100 %
   :align: center


To use the GNSS INS Evaluation Kit, connect the hardware as follows: 

1. Connect breakout cable to GNSS INS unit
2. Connect to power using either the AC/DC adapter or the Auto Cable Plug.
3. Connect primary GNSS antenna to ANT1 using SMA to FAKRA Adapters. An additional antenna (ANT2) is optional and enables stationary dual heading.
4. Connect to PC, Mac, or Ubuntu computing system via RS-232 using USB 2.0 to DB9 Serial Converters for configuration.
5. If you'd like to use Automotive Ethernet, see section 4.1 for connection instructions.

For more information on hardware mechanicals, see `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html#anello-gnss-ins>`__.


2   Unit Configurations
---------------------------------
If you would like to use the ANELLO Python Program to connect, configure, and log data with the GNSS INS, please use the following instructions.

2.1 Install ANELLO Python Program
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Confirm that Python is installed on your computer and the version is at least 3.6:

.. code-block:: python
    
    >python -V

Clone the GitHub repository:

.. code-block:: python

    git clone https://github.com/Anello-Photonics/user_tool.git

Install dependencies using pip:

.. code-block:: python
    
    >cd user_tool
    >pip install -r requirements.txt

If you have any errors with these steps, see `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/latest/setup_troubleshooting.html#install-anello-python-program>`__.

Please run "git pull" periodically to make sure you are using the latest code.

2.2 Run the Python Tool 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python
    
    >cd board_tools
    >python user_program.py

You will see *System Status* at the top, and *Main Menu* below. For more information, see `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/latest/setup_troubleshooting.html#run-python-program>`__.

2.3 Connect to the GNSS INS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Use the arrow keys to select *Connect*, then *COM*, then *Auto* to auto-detect the unit.  You can also use *Manual* if you know the data and config ports.
You should now see the *System Status* updated with the device information.

For more information or if you experience any errors, see the `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/latest/setup_troubleshooting.html#connect-to-evk>`__.

2.4 GNSS INS Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Select *Unit Configuration* from the main menu to see default configurations. To change any configurations, 
select *Edit*, then the configuration to change, then select the new value.

For more information, please see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_.


3   Data Collection
---------------------------------
3.1 Log a Data File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In the main menu, select *Log*, then *Start*. Use the default filename or enter a custom name. 
The *System Status* will be updated with the logging information.

To end a log, select *Log* then *Stop*. Log files are saved in the "logs" directory in user_tool, 
grouped by month and day.

To export a log to CSV, Select *Log*, then *Export*, then choose the log file.
CSV files for each message (IMU, GPS, HDG, and INS) will be saved in the "exports" directory, under the name of the original log file. 
For more information on the output messages, see `Comminication & Messaging <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html>`_.

INS solution can be visualized by importing ins.csv into `Kepler <https://kepler.gl/demo>`_

3.2 Monitor Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For a real-time display of the INS solution, select *Monitor* in the main menu.

Logging can be started and ended by clicking the LOG button, and GNSS input can be turned on or off by clicking the GPS button.

3.3 Connect to NTRIP Caster
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Connecting to NTRIP will improve the GNSS position accuracy by using RTK corrections.

From the main menu, select *NTRIP* and then *Start*. Enter the NTRIP caster details as prompted. 
The *System Status* will show the NTRIP connection status.


4   Vehicle Installation
-------------------------------
4.1 Connect via Automotive Ethernet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The GNSS INS automotive ethernet interface is recommended for in-vehicle data collection. If your system does not use automotive ethernet, a media converter such as the `Rad Moon <https://intrepidcs.com/products/automotive-ethernet-tools/rad-moon-100base-t1-ethernet-media-converter/>`_ would be reequired.

1. Find Computer Ethernet IP using ipconfig in terminal
2. In user_program.py, select *Unit Configurations*
   
   - Set Computer IP to that from step 1
   - Keep data and configuration port as 1111 and 2222 (these can be any number not used for something else, e.g. your OS)
   
   If connecting directly to computer:
   
   - Set DHCP to off
   - Set GNSS INS (A1) IP to something with same prefix as Computer IP
   
   If connecting GNSS INS to computer through router:
   
   - Set DHCP on
   - GNSS INS (A1) IP will be auto-assigned after restart

3. Restart GNSS INS and re-connect via RS-232
4. In main menu, select *Unit Configurations*, take note of GNSS INS IP and data/config ports
5. In main menu, select *Connect* -> *UDP* -> Enter GNSS INS (A1) IP and data/config ports


4.2 Install the GNSS INS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The GNSS INS can be configured for various installation positions. To minimize configuration steps, 
mount near the center of the vehicle’s rear axle, with the x-axis facing the direction of travel.

.. image:: media/GNSSINS_Vehicle_Installation.png
   :width: 50 %
   :align: center

The GNSS antennae can be magnetically mounted on the roof of the vehicle.

4.3 Set Vehicle Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In main menu, select *Vehicle Configurations* and set the lever arms as prompted. For more informaiton, see `Vehicle Configurations <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html>`_.

**Congratulations!!!**
You have completed the GNSS INS setup! Please refer back to `Section 3 <https://docs-a1.readthedocs.io/en/latest/getting_started_quick.html#data-collection>`_ for data collection. 
Note that the GNSS INS performance will improve after several minutes of driving.

Please contact support@anellophotonics.com with any questions. 