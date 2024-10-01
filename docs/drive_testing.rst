==================================
Drive Testing Best Practices
==================================
Please review these best practices prior to drive testing. 
Note that the EVK and GNSS INS algorithms are currently optimized for wheeled land vehicles. 
For support on other vehicle types, please contact support@anellophotonics.com.

**Important:** If you would like help from the ANELLO team on data analysis and fine-tuning, we ask that you please share:

1. The raw log file starting at power-up and including the entire drive period
2. The config file (using the "Save Configs" option in the Python tool main menu). If you do not see this option, please run "git pull" to make sure you have the latest code.
3. Pictures of the installation (ANELLO unit and antennae)
4. Description of test vehicle and testing goals


Vehicle Configurations
---------------------------------
It is important to ensure the lever arms are set properly prior to drive testing. 
If you are using dual antenna in your setup, this includes an accurate antenna baseline calibration.

For more information, please refer to `Vehicle Configurations <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html>`_.


Odometer Input
-----------------------
For extended GNSS-denied testing, it is strongly recommended to add odometer input to the ANELLO EVK or GNSS INS to minimize error, particularly in distance traveled.


Initialization
-----------------------
There are a couple important notes to optimize the performance of your ANELLO EVK. 

Heading Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Heading can be initialized in one of two ways: 

1. Using GNSS velocity heading (while moving). This requires driving forwards at speeds of > 2 m/s (> 5 mph) for about 30 seconds. The faster you drive, the better tuned the GNSS velocity heading will be.
2. Using dual antenna heading (while stationary). This requires an antenna separation of at least 0.6 meters, and accurate antenna baseline calibration (see `Vehicle Configurations <https://docs-a1.readthedocs.io/en/latest/vehicle_configuration.html>`_).

Direction Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
At the start of the drive, if the heading has not been initialized using dual antenna heading and odometer input with directionality is not provided to the unit, 
the algorithm currently assumes the vehicle direction is forwards.

To support forward and backward initialization, ANELLO recommends one of the following: 

1. Provide odometer input to indicate forwards/backwards speeds
2. Ensure dual antenna heading is initialized prior to vehicle movement

Filter Initialization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Kalman filter typically converges within 2-5 minutes of driving. 
The faster the driving, and the more dynamics the vehicle experiences (e.g. left and right turns), the faster the filter converges.

For GPS-denied testing, we recommend allowing at least a 2-5 minute initialization period of driving prior to GPS signal loss.