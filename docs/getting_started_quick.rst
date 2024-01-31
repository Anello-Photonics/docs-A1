==================================
EVK Getting Started Guide
==================================
Thank you for choosing the ANELLO EVK! This guide will get you started with EVK connection, configuration and data collection.
Please contact support@anellophotonics.com with any questions.  

Hardware Connections
---------------------------------
The ANELLO Evaluation Kit (EVK) includes the following items:

    +---+------------------------------------------------+
    | 1 | ANELLO EVK                                     |
    +---+------------------------------------------------+
    | 2 | Two Dual-Band Multi-Constellation GNSS Antennae|
    +---+------------------------------------------------+
    | 3 | Power Cable                                    |
    +---+------------------------------------------------+
    | 4 | 110-240V AC Wall-Power Adapter                 |
    +---+------------------------------------------------+
    | 5 | International Wall-Power Plug Inserts          |
    +---+------------------------------------------------+
    | 6 | In-Vehicle Power Adapter                       |
    +---+------------------------------------------------+
    | 7 | USB Cable                                      |
    +---+------------------------------------------------+
    | 8 | Ethernet Cable                                 |
    +---+------------------------------------------------+

.. image:: media/evk_contents.png
   :width: 90 %
   :align: center


Connect the hardware as follows: 

1. Connect EVK to power using either the wall-power or the in-vehicle adapter (red).
2. Connect EVK to computer using USB-C (blue). Ethernet interface (green) is also available, but connection over serial is required first to configure IP addresses.
3. Connect primary GNSS antenna to ANT1 on the back of the EVK (black). An optional additional antenna (ANT2) enables stationary heading initialization.

.. image:: media/EVK-wiring_2.png
   :width: 100 %
   :align: center

For more information on hardware mechanicals, see `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html#anello-evk>`_.


Software Interfaces
---------------------------------
ANELLO provides a Python tool to connect, configure, and log data with the EVK.
Please see instructions on `ANELLO Python Tool <https://docs-a1.readthedocs.io/en/latest/python_tool.html>`__.

ANELLO units are also compatible with ROS using our C-based `ROS driver <https://github.com/Anello-Photonics/ANELLO_ROS_Driver>`_.

If you would like to connect to the EVK using a serial interface software such as CoolTerm, 
please ensure you use the correct baud rate (default for the EVK is 921600), and set Data Bits = 8, Stop Bits = 1, and Parity = None.

For a full list of software tools, please see `Software Tools <https://docs-a1.readthedocs.io/en/latest/software_tools.html>`_.


Configure ANELLO EVK
---------------------------------
Before testing your EVK, please review the available configurations and ensure they are set according to your testing needs.
A description of ANELLO unit and vehicle configurations can be found at `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_,
and `Vehicle Configurations <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html>`_, respectively.

There are two options to change configurations:

Configure using ANELLO Python Tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For unit configurations, select *Unit Configuration* from the main menu to see default configurations. To change a configuration, 
select *Edit*, then the configuration to change, then select or enter the new value.

For vehicle configurations, select *Vehicle Configuration* from the main menu and select *Edit*, to set the lever arms.

Note that as of firmware v1.2.0, to use dual antenna functionality, the antenna baseline must be calibrated. Please refer to 
`Vehicle Configurations <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html>`_ to ensure all vehicle configurations are set properly.

Other Configuration Methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You may also send configurations manually over the EVK's serial configuration port using a serial interface software, such as CoolTerm.
Note that the configuration port on the EVK is the highest of the four virtual COM ports (more information at `Comminication & Messaging <https://docs-a1.readthedocs.io/en/latest/communication_messaging.html>`_). 


Connect to NTRIP Caster
------------------------------
Standard RTCM messages can be forwarded to the ANELLO EVK data port to enable the GNSS receivers to reach RTK-level accuracy. 
The EVK receives standard RTCM3.3 in MSM format, including MSM4, MSM5, and MSM7 messages. 

The ANELLO Python Program also provides an NTRIP client which can connect to a standard NTRIP network and forward the received RTCM messages into the EVK.

From the main menu, select *NTRIP* and then *Start*. Enter the NTRIP caster details as prompted. 
The *System Status* will show the NTRIP connection status.


Vehicle Installation
----------------------------

The EVK can be configured for various installation positions. To minimize configuration steps, 
mount near the center of the vehicle’s rear axle, with the x-axis facing the direction of travel.

.. image:: media/a1_install_location.png
   :width: 50 %
   :align: center

The GNSS antennae can be magnetically mounted on the roof of the vehicle.


Data Collection & Visualization
------------------------------------

Log Data
~~~~~~~~~~~~~~~~~
To log data, you may use the ANELLO Python Tool, the ANELLO ROS driver, or another program of your choice.

To maximize ANELLO's ability to help analyze your data, we recommend logging data with the ANELLO Python Tool. Instructions can be found at 
`ANELLO Python Tool <https://docs-a1.readthedocs.io/en/latest/python_tool.html#data-collection>`__.

Monitor Data Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For a real-time display of the ANELLO EVK data, select *Monitor* in the main menu.

More information on the monitor window can be found at `ANELLO Python Tool <https://docs-a1.readthedocs.io/en/latest/python_tool.html#monitor-output>`__.

Data Visualization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
INS solution can be visualized by importing ins.csv into `Kepler <https://kepler.gl/demo>`_.
You may also use a `CSV to KML converter <https://www.convertcsv.com/csv-to-kml.htm>`_ to visualize the results in Google Earth, 
but note that these tools often have data length limitations.


Drive Testing
-------------------
Before conducting drive testing, please review `Drive Testing Best Practices <https://docs-a1.readthedocs.io/en/latest/drive_testing.html>`_ 
to ensure the system is set up properly, initializes smoothly, and is optimized for your use case.

**Congratulations!!!**
You have completed the EVK setup and data collection! Please feel free to contact support@anellophotonics.com with any questions. 