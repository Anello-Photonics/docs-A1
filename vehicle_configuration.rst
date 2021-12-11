Vehicle Configurations
=======================

Vehicle configurations describe the positions of parts in the vehicle setup, which are used in the INS message calculations. The IMU message is not affected.
All positions are x,y,z triples in meters, using the A1 as the origin and in the A1's coordinate system.

These are configurable with the Anello Python Program by selecting "VEHICLE CONFIGURATIONS" in the main menu.
They can also be set directly by **#APVEH** messages over usb or ethernet connection.

+----------------+------------------+---------------------------------------------------------+
| Configuration  | codes(for APVEH) |                     Description                         |
+----------------+------------------+---------------------------------------------------------+
|  Lever Arm 1   |  g1x, g1y, g1z   |   GNSS Antenna 1 position relative to A1                |
+----------------+------------------+---------------------------------------------------------+
|  Lever Arm 2   |  g2x, g2y, g2z   |   GNSS Antenna 2 position relative to A1                |
+----------------+------------------+---------------------------------------------------------+
| Vehicle Center |  cnx, cny, cnz   |   Center of rear wheels, origin for odometer input      |
+----------------+------------------+---------------------------------------------------------+
| Output Center  |  ocx, ocy, ocz   |   Origin for positioning in INS output                  |
+----------------+------------------+---------------------------------------------------------+
