Known Issues and Limitations
==============================

* If no odometer input is sent into the unit, initialization of system while driving backwards is not currently supported.

* The algorithm on the ANELLO GNSS INS and EVK is currently designed for wheeled land vehicles. Contact ANELLO about availability and support for other vehicle types such as aerial and maritime vehicles.

* The CAN interface is not yet available. Please use the serial and ethernet interfaces and contact ANELLO for questions on future CAN availability.

* The ANELLO binary message does not work on units delivered before April 13, 2023. Please contact ANELLO for assistance if this is needed.

* The static dual antennae heading initialization feature is only available on units shipped after Nov 1, 2022 and with firmware version 1.0 or later. If you received your unit prior to this date and are interested in this feature, please contact ANELLO.

* Units with firmware versions before v1.1 may experience some timing jitter on the IMU and INS timestamps. For improved timing performance, please contact ANELLO to upgrade to the latest firmware.

* There is some timing jitter expected in the GPS, GP2, and HDG packets. 