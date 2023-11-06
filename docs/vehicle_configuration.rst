Vehicle Configuration
=======================

Vehicle configurations describe the positions of parts in the vehicle setup, which are used in the ANELLO algorithm calculations. The IMU message is not affected by these settings.
All positions are x,y,z in meters, using the EVK as the origin and using the standard EVK coordinate system (not corrected for mounting orientation).

These are configurable with the ANELLO Python Program by selecting *Vehicle Configurations* in the main menu.
They can also be set directly by **#APVEH** messages over USB or Ethernet connection. 

+----------------+------------------+------------------------------------------------------------+
| Configuration  | APVEH Codes      |                     Description                            |
+----------------+------------------+------------------------------------------------------------+
|  Lever Arm 1   |  g1x, g1y, g1z   |   Vector from EVK center to antenna 1                      |
+----------------+------------------+------------------------------------------------------------+
|  Lever Arm 2   |  g2x, g2y, g2z   |   Vector from EVK center to antenna 2                      |
+----------------+------------------+------------------------------------------------------------+
| Vehicle Center |  cnx, cny, cnz   |   Vector from EVK center to center of rear axle            |
+----------------+------------------+------------------------------------------------------------+
| Output Center  |  ocx, ocy, ocz   |   Vector from EVK center to desired INS solution position  |
+----------------+------------------+------------------------------------------------------------+

The mounting location of the EVK is flexible and can be configured for various installation positions and orientations. 
For the least complexity, we recommendation mounting the unit above the center of the vehicle’s rear axle with the X-Axis facing forward along the direction of travel. 

.. figure:: media/a1_install_location.png
   :scale: 50 %
   :align: center

   Default EVK Installation Location

If the unit is oriented differently, the orientation must be configured using the ANELLO Python Program, 
see `Unit Configurations <https://docs-a1.readthedocs.io/en/latest/unit_configuration.html>`_.

The antennae provided with the ANELLO EVK magnetically mount to the vehicle roof. Alternative GNSS antennae can be 
used, as long as they support dual band and all constellations. Please avoid placing antennae on significantly cureved surfaces 
or below any obstructions which would reduce available sky view.

EVKs received after Nov 1, 2022 and with firmware version 1.0 or later will have capability for 
dual GNSS static heading initialization. If using dual GNSS, the primary antenna needs to be in back, 
secondary antenna in front. They should be aligned to the center of the vehicle frame at least 0.6 meters separated.

.. note:: The EVK algorithm is currently intended for wheeled land vehicles. Contact ANELLO about availability and support for 
   other vehicle types such as aircraft/drones, marine vehicles, and tracked land vehicles.