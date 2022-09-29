Vehicle Configuration
=======================

Vehicle configurations describe the positions of parts in the vehicle setup, which are used in the INS message calculations. The IMU message is not affected.
All positions are x,y,z triples in meters, using the EVK as the origin and using the standard EVK coordinate system (not corrected for mounting orientation).

These are configurable with the Anello Python Program by selecting "VEHICLE CONFIGURATIONS" in the main menu.
They can also be set directly by **#APVEH** messages over usb or ethernet connection. 

+----------------+------------------+---------------------------------------------------------+
| Configuration  | codes(for APVEH) |                     Description                         |
+----------------+------------------+---------------------------------------------------------+
|  Lever Arm 1   |  g1x, g1y, g1z   |   GNSS Antenna 1 position relative to EVK               |
+----------------+------------------+---------------------------------------------------------+
|  Lever Arm 2   |  g2x, g2y, g2z   |   GNSS Antenna 2 position relative to EVK               |
+----------------+------------------+---------------------------------------------------------+
| Vehicle Center |  cnx, cny, cnz   |   Center of rear wheels, origin for odometer input      |
+----------------+------------------+---------------------------------------------------------+
| Output Center  |  ocx, ocy, ocz   |   Origin for positioning in INS output                  |
+----------------+------------------+---------------------------------------------------------+

The EVK is easy to install on a land vehicle. The mounting location of the EVK is flexible and can be configured for various 
installation positions and orientations. For getting starting quickly and minimizing the configuration steps, 
the recommendation is to mount the unit nearer the vehicle’s rear axle and along the vehicle 
centerline with the X-Axis facing forward along the direction of travel.  This mounting location will ensure 
good results with minimal configuration.


.. figure:: media/a1_install_location.png
   :scale: 50 %
   :align: center

   Default EVK Installation Location

If the unit is oriented differently, then the orientation (“orn”) setting must be configured using the Anello 
Python Program.

See Advanced configuration and Anello Python Program detailed descriptions.

The GNSS antennae should be placed on the roof of the vehicle. The primary GNSS antenna is labelled GPS1 on 
the back of the EVK.  GPS1 must be connected for proper system operation.  GPS2 is optional. 

If the primary antennae is placed directly above the IMU, this results in the simplest lever-arm configuration. 
**Avoid** placing the antennae on significantly curved surfaces as 
this will reduce the available sky view of the Antennae.  **Do not place the antennae inside the vehicle or 
underneath other things (especially metal) as this will significantly reduce the GNSS signal quality.**

The antennae provided in the Anello EVK magnetically mounts to the vehicle roof.  Alternative GNSS antennae can be 
used, so long as they support *BOTH GPS L1 and L2C bands as well as the equivalent signals on the 
Glonass, Galileo, and Beidou constellations*.  Contact Anello if there are questions about using an alternate 
GNSS antennae.  

Finally, the EVK ships with an in-vehicle power adapter.  If the in-vehicle power adapter is used, please ensure the plug is 
securely and fully inserted.   Alternatively, cut-off in-vehicle power adapter and connect the red and black power lines 
to a stable source of power in the range of 8 to 30 VDC.

Once the Anello EVK is properly installed in the vehicle, you are ready to collect data.  Unlike other 
systems, the EVK does not require an extensive driving calibration prior to usage.  However, the 
system does require exceeding 2m/s velocity to enter full INS mode, and the performance will generally improve after 
the first 5 minutes of driving.

.. note::

    Use of a single band (L1 only) GNSS antennae will result in a significant reduction of accuracy and 
    likely prevent RTK from working at all. Please ensure the antennae has at least dual-band support.

    A known limitation of the initial EVK unit is that it is intended for wheeled land vehicles.  Contact 
    Anello about availability and support for other vehicle types such as aircraft/drones, marine vehicles, 
    and tracked land vehicles

    The initial release supports logging data from both GNSS antennas, but does not include GNSS static heading 
    initialization.

