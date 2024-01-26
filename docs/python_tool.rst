ANELLO Python Tool
=====================

The ANELLO Python Program can be used to connect, configure, and log data with any ANELLO product. 
It can also be used as an NTRIP client to forward RTK corrections to your EVK or GNSS INS for enhanced GPS accuracy.
Please see the instructions below to use the ANELLO Python program and reach out to us with any questions.

1   Install ANELLO Python Program
-------------------------------------

Confirm that `python <https://www.python.org/downloads/>`_ is installed on your computer and the version is at least 3.6:

.. code-block:: python
    
    python --version

.. note::
    If the above shows python 2.x despite Python 3 being installed, try "python3 --version". 
    If that shows python 3.x, please use "python3" instead of "python" when running user_program.py from the command line.

Make sure you have a `git client <https://git-scm.com/download>`_ on your computer as this is the easiest way to stay up-to-date with the ANELLO Python Program.

Clone the GitHub repository:

.. code-block:: python

    git clone https://github.com/Anello-Photonics/user_tool.git

.. note::
    Please run "git pull" regularly to make sure you are using the latest Python tool features.

Install dependencies using pip:

.. code-block:: python
    
    cd user_tool
    pip install -r requirements.txt

If you have any issues, see `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/latest/setup_troubleshooting.html#install-anello-python-program>`__.

2   Run the Python Tool 
-------------------------------------

.. code-block:: python
    
    cd board_tools
    python user_program.py

You will see *System Status* at the top, and *Main Menu* below. For more information, see `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/latest/setup_troubleshooting.html#run-python-program>`__.

3   Connect to ANELLO Unit
-------------------------------------

3.1 Connect Over Serial
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Ensure the power cable is connected and the green power light is illuminated. Connect the ANELLO unit to the computer using the USB-C interface.

Use the arrow keys to select *Connect*, then *COM*, then *Auto* to auto-detect the unit. You can also use *Manual* if you know the data and config ports.
You should now see the *System Status* updated with the device information.

For more information or if you experience any errors, see the `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/latest/setup_troubleshooting.html#connect-to-anello-unit>`__.

3.2 Connect Over Ethernet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Ethernet (UDP) interface is recommended for in-vehicle data collection. 

1. Find Computer Ethernet IP using ipconfig in terminal
2. In user_program.py, select *Unit Configurations*
   
   - Set Computer IP to that from step 1
   - Keep data and configuration port as 1111 and 2222 (these can be any number not  used for something else, e.g. your OS)
   
   If connecting the unit directly to computer:
   
   - Set DHCP to off
   - Set ANELLO IP to something with same prefix as Computer IP
   
   If connecting the unit to computer through router:
   
   - Set DHCP on
   - ANELLO IP will be auto-assigned after restart

3. Restart the unit (either using Python program or manually power cycling the unit) and re-connect via COM
4. In main menu, select *Unit Configurations*, take note of ANELLO IP and data/config ports
5. In main menu, select *Connect* -> *UDP* -> Enter ANELLO IP and data/config ports

4   Set ANELLO Configurations
-------------------------------------

4.1 Unit Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In main menu, select *Unit Configuration* to see default configurations. To change any configurations, 
select *Edit*, then the configuration to change, then select the new value.

Please see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_ for more information on available configurations.

4.2 Vehicle Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Before you collect data with the ANELLO EVK of GNSS INS, vehicle configurations must be set.

In main menu, select *Vehicle Configurations* and set the lever arms as prompted. 
All measurements are set using the center of the ANELLO unit as the origin and are measured in meters.

For firmware versions 1.2.0 and later, antenna baseline calibration must be performed. 

Please see `Vehicle Configurations <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html>`_ for more information.

5   Data Collection
---------------------------------

In the main menu, select *Log*, then *Start*. Use the default filename or enter a custom name. 
The *System Status* will be updated with the logging information.

To end a log, select *Log* then *Stop*. Log files are saved in the "logs" directory in user_tool, grouped by month and day.

To export a log to CSV, Select *Log*, then *Export to CSV*, then choose the log file.
CSV files for each message (IMU, GPS, GP2, HDG, and INS) will be saved in the "exports" directory, under the name of the original log file. 
For more information on the output messages, see `Comminication & Messaging <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html>`_.

The INS solution can be visualized by importing ins.csv into `Kepler <https://kepler.gl/demo>`_.

6   Monitor Output
-------------------------------------
For a real-time display of the ANELLO data, select *Monitor* in the main menu.

Logging can be started/ended by clicking the LOG button, and GNSS input can be turned on/off by clicking the GPS button.
If the LOG button is red, that means data is not logging, and if the GPS button is red, GNSS input is turned off.

Turning the GPS button off stops sending GPS data into the ANELLO unit, and may be useful for simulating GPS-denied scenarios without physically blocking GPS signal. 
You may also simulate GPS loss by covering antennae with an metal enclosure, using a digital attenuator, or other methods.
ANELLO does not recommend simulating GPS loss by disconnecting antennae mid-drive as this can often cause spurious signal to be read by the GPS receiver and fed into the INS algorithm.

7   Connect to NTRIP Caster
-------------------------------------
Standard RTCM messages can be forwarded to the ANELLO EVK and GNSS INS units data port to enable the GNSS receivers to reach RTK-level accuracy. 
The EVK and GNSS INS receive standard RTCM3.3 in MSM format, including MSM4, MSM5, and MSM7 messages. 

The ANELLO Python Program also provides an NTRIP client which can connect to a standard NTRIP network and forward the received RTCM messages into the EVK.

From the main menu, select *NTRIP* and then *Start*. Enter the NTRIP caster details as prompted. 
The *System Status* will show the NTRIP connection status.