=========================
Sensor Recommendations
=========================

The ANELLO Ground INS Gen 2 has been successfully integrated with a variety of speed sensors, GNSS antennas, and M-Code receivers. The following recommendations are based on tested integrations and typical maritime deployment requirements.


GNSS Antenna Recommendations
----------------------------

ANELLO recommends using active L1/L2/L5 all-constellation GNSS antennas. The following models have been successfully integrated with the Ground INS Gen 2.

u-blox ANN-MB2
^^^^^^^^^^^^^^

A cost-effective all-band active GNSS antenna featuring:

* 5 m SMA cable
* Magnetic or screw mounting options
* IP67 environmental rating

Recommended for:

* Rapid integration efforts
* Cost-sensitive programs

Tallysman TW3972
^^^^^^^^^^^^^^^^

A rugged triple-band GNSS antenna featuring:

* Permanent-mount design
* IP69K environmental rating
* L-band correction support
* Low current consumption
* Strong RF filtering

Recommended for:

* General-purpose maritime deployments
* Ruggedized installations

Calian VeroStar VSP6337L
^^^^^^^^^^^^^^^^^^^^^^^^

A high-performance marine GNSS antenna featuring:

* IP69K environmental rating
* IEC 60945 and IEC 61108 compliance
* Low phase-center variation
* Strong low-elevation satellite tracking
* Iridium and Inmarsat interference rejection

Recommended for:

* High-performance marine navigation systems
* Challenging RF environments

Antenna Installation Guidelines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* The Ground INS Gen 2 supplies 3.3 V bias power for active antennas
* The antenna baseline must be at least 1 meter; longer baselines improve dual-antenna heading accuracy
* Ensure clear sky visibility for both antennas
* Follow any ground-plane requirements specified by the antenna manufacturer
* Use the same antenna model and identical cable type and length for ANT1 and ANT2 unless otherwise validated

M-Code Receiver Recommendations
-------------------------------

The Ground INS Gen 2 has been successfully integrated with the following external M-Code receivers.

Supported Receivers
^^^^^^^^^^^^^^^^^^^

* Collins NavHub 100
* SFS Valiant Splash

Integration Requirements
^^^^^^^^^^^^^^^^^^^^^^^^

The Ground INS Gen 2 accepts M-Code aiding as external GNSS position and velocity aiding through the receiver's decoded NMEA 0183 output.

Minimum required NMEA 0183 messages:

* RMC
* GGA
* GSA

Requirements:

* Message update rate of at least 0.5 Hz
* RMC must indicate active status
* Position, date, and time fields must be valid
* GSA must report a valid 3D fix with finite PDOP, HDOP, and VDOP values

Supported Interfaces
^^^^^^^^^^^^^^^^^^^^

* RS-232
* Ethernet using NMEA UDP

RS-232 direct connection is the most common integration method.

Operational Guidance
^^^^^^^^^^^^^^^^^^^^

* Use keyed M-Code operation whenever available
* Treat unkeyed or fallback modes as lower-assurance aiding sources

For any questions or concerns regarding external sensors, contact support@anellophotonics.com