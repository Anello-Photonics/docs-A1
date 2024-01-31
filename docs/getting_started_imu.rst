==================================
IMU & IMU+ Getting Started Guide
==================================
Thank you for choosing the ANELLO IMU/IMU+! This guide will get you started with connection, configuration and data collection.
Please contact support@anellophotonics.com with any questions.  

Hardware Connections
---------------------------------
The ANELLO IMU unit is pictured below. It features an 8 pin automotive-grade Molex MX150 connector.

.. image:: media/ANELLO_IMU.png
   :width: 50 %
   :align: center

If you purchased the IMU Evaluation Kit, you will also receive the items pictured below. See `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html>`__ for schematic of the breakout cable.

.. image:: media/IMU_EvalKit.png
   :width: 100 %
   :align: center


To use the IMU Evaluation Kit, connect the hardware as follows: 

1. Connect breakout cable to IMU unit
2. Connect to power using the AC/DC adapter.
3. Connect to PC, Mac, or Ubuntu computing system via RS-232 using USB 2.0 to DB9 Serial Converters.

For more information on hardware mechanicals, see `Mechanicals <https://docs-a1.readthedocs.io/en/latest/mechanicals.html#anello-imu-imu>`__.


Software Interfaces
---------------------------------
ANELLO provides a Python tool to connect, configure, and log data with the ANELLO IMU.
Please see instructions on `ANELLO Python Tool <https://docs-a1.readthedocs.io/en/latest/python_tool.html>`__.

ANELLO units are also compatible with ROS using our C-based `ROS driver <https://github.com/Anello-Photonics/ANELLO_ROS_Driver>`_.

If you would like to connect to the IMU using a serial interface software such as CoolTerm, 
please ensure you use the correct baud rate (default for the IMU is 230400), and set Data Bits = 8, Stop Bits = 1, and Parity = None.

For a full list of software tools, please see `Software Tools <https://docs-a1.readthedocs.io/en/latest/software_tools.html>`_.


Configure ANELLO IMU/IMU+
---------------------------------
Before testing your IMU, please review the available configurations and ensure they are set according to your testing needs.
A description of ANELLO unit and vehicle configurations can be found at `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_..

There are two options to change configurations:

Configure using ANELLO Python Tool
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For unit configurations, select *Unit Configuration* from the main menu to see default configurations. To change a configuration, 
select *Edit*, then the configuration to change, then select or enter the new value.

Other Configuration Methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You may also send configurations manually over the IMU's serial configuration port using a serial interface software, such as CoolTerm.


Data Collection & Visualization
------------------------------------

Log Data
~~~~~~~~~~~~~~~~~
To log data, you may use the ANELLO Python Tool, the ANELLO ROS driver, or another program of your choice.

To maximize ANELLO's ability to help analyze your data, we recommend logging data with the ANELLO Python Tool. Instructions can be found at 
`ANELLO Python Tool <https://docs-a1.readthedocs.io/en/latest/python_tool.html#data-collection>`__.

Monitor Data Output
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
For a real-time display of the ANELLO IMU data, select *Monitor* in the main menu.

More information on the monitor window can be found at `ANELLO Python Tool <https://docs-a1.readthedocs.io/en/latest/python_tool.html#monitor-output>`__.


**Congratulations!!!**
You have completed the IMU setup! Please contact support@anellophotonics.com with any questions. 