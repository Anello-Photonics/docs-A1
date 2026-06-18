=========================
Sensor Recommendations
=========================

The ANELLO Maritime INS has been successfully integrated with a variety of speed sensors, GNSS antennas, and M-Code receivers. The following recommendations are based on tested integrations and typical maritime deployment requirements. :contentReference[oaicite:0]{index=0}

Speed Sensors
-------------

The Maritime INS requires a speed sensor to provide aiding data for optimal navigation performance. ANELLO recommends selecting a sensor that supports the full operating speed range of the vessel. :contentReference[oaicite:1]{index=1}

Paddle Wheel
^^^^^^^^^^^^

**Recommended Model:** Airmar DST-810 (NMEA 2000)

Advantages:

* Most cost-effective and widely used option for unmanned surface vessels (USVs)
* Provides continuous water-speed measurements, including during brief vessel planing events

Installation Notes:

* Available in both thru-hull and transom-mount configurations
* Thru-hull installation is recommended for best performance

Ultrasonic
^^^^^^^^^^

**Recommended Model:** Airmar UDST-800 (NMEA 2000)

Advantages:

* No moving parts, improving long-term reliability
* Can provide improved accuracy at low speeds

Considerations:

* May lose valid speed measurements when the vessel is planing at high speed

Doppler Velocity Log (DVL)
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Recommended Model:** Nortek Nucleus 1000

Interface:

* RS-232 with Nortek proprietary output

Advantages:

* Provides ground-referenced velocity when bottom lock is available (typically within 50–100 m of the seabed)
* Can also provide speed-through-water measurements
* Best choice for extended GPS-denied operation when bottom lock can be maintained, such as on UUVs

Considerations:

* Maximum measurable speed is approximately 10 knots

Speed Sensor Installation Guidelines
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* Ensure the selected sensor supports the full speed range of the vessel
* Follow the manufacturer's installation guidelines
* Mount the sensor near the centerline of the hull whenever possible
* Avoid obstructions that could disrupt water flow across the sensing element

GNSS Antenna Recommendations
----------------------------

ANELLO recommends using active L1/L2/L5 all-constellation GNSS antennas. The following models have been successfully integrated with the Maritime INS. :contentReference[oaicite:2]{index=2}

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

* The Maritime INS supplies 3.3 V bias power for active antennas
* The antenna baseline must be at least 1 meter; longer baselines improve dual-antenna heading accuracy
* Ensure clear sky visibility for both antennas
* Follow any ground-plane requirements specified by the antenna manufacturer
* Use the same antenna model and identical cable type and length for ANT1 and ANT2 unless otherwise validated

M-Code Receiver Recommendations
-------------------------------

The Maritime INS has been successfully integrated with the following external M-Code receivers. :contentReference[oaicite:3]{index=3}

Supported Receivers
^^^^^^^^^^^^^^^^^^^

* Collins NavHub 100
* SFS Valiant Splash

Integration Requirements
^^^^^^^^^^^^^^^^^^^^^^^^

The Maritime INS accepts M-Code aiding as external GNSS position and velocity aiding through the receiver's decoded NMEA 0183 output.

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

For additional information on external position aiding, see the Maritime INS communication and messaging documentation.