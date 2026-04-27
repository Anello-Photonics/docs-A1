==================================
Sensor Calibrations
==================================


(Optional) External Magnetometer Calibration
----------------------------------------------

In the case that you will be supplying the ANELLO Aerial INS with data from an external magnetometer via `NMEA0183 VHW input <https://docs-a1.readthedocs.io/en/aerial_ins/communication_messaging.html#vhw-water-speed-heading>`_ or the ANELLO Proprietary Auxiliary RPH messages in `NMEA0183 <https://docs-a1.readthedocs.io/en/aerial_ins/communication_messaging.html#paprph-roll-pitch-heading-with-accuracies-anello-proprietary>`_ or `NMEA2000 <https://docs-a1.readthedocs.io/en/aerial_ins/communication_messaging.html#pgn-130817-auxiliary-attitude-anello-proprietary>`_ you may conduct an optional external magnetometer calibration procedure to improve the accuracy of the reported heading. 

 

Calibration procedures will seek to gain visibility into all potential headings and motor percentages during operation. If you will be using an electric motor, it is recommended to limit the amount of ramping of the motor during the data collection period, to minimize abrupt changes in the induced magnetic field of the motor. 

 

To calibrate the external magnetometer: 

1. Launch boat into water. 

2. Power on the ANELLO Aerial INS while boat is stationary and allow ~30s to gain GNSS. 

3. Perform a “figure-eight” or “flower” shaped mission at a series of increasing motor percentages, maintaining a constant motor percentage per each figure-eight or flower.  

4. Once the calibration mission is complete, power off the ANELLO Aerial INS. 

5. After you finish other water missions, download the logs from the ANELLO Aerial INS (using the instructions under `Data Collection and Visualization <https://docs-a1.readthedocs.io/en/aerial_ins/getting_started_aerialins.html#data-collection-visualization>`__ ) and send to ANELLO.

6. ANELLO will instruct on updating the external magnetometer calibration values. 

 

**Re-calibration of the external magnetometer is necessary if any of the following conditions are met:** 

* The boat type/model is changed. 

* The external magnetometer type/model is changed. 

* The location of the external magnetometer is changed. 

* The motor type/model is changed. 

* The magnetic components of the boat are significantly changed. 