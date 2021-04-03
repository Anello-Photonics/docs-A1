Advanced Configuration
=======================

There following advanced settings are configurable with the Anello Python Program or **#APCFG** messages.

+-----------+----------------------------------------------------------------------------+
|  Param    | Value / Description                                                        |
+-----------+----------------------------------------------------------------------------+
|  orn      |  e.g. **+X-Y-Z** or any of the possible 24 right-handed frame options      |
+-----------+----------------------------------------------------------------------------+
|  odr      |  output data rate of primary messages **20,50,100,200** Hz                 |
+-----------+----------------------------------------------------------------------------+
|  gps1     |  "on", "off"                                                               |
+-----------+----------------------------------------------------------------------------+
|  gps2     |  "on", "off"                                                               |
+-----------+----------------------------------------------------------------------------+
|  odo      |  odometer "on", "off", "mps", "mph", "kph", "fps"  (See Note)              |
+-----------+----------------------------------------------------------------------------+
|  dhcp     |  DHCP "on", "off"                                                          |
+-----------+----------------------------------------------------------------------------+
|  lip      | local IP (aaa.bbb.ccc.ddd)                                                 |
+-----------+----------------------------------------------------------------------------+
|  rip      | remote IP (aaa.bbb.ccc.ddd)                                                |
+-----------+----------------------------------------------------------------------------+
|  rport1   | remote port 1 (data port) integer from 1 to 65535                          |
+-----------+----------------------------------------------------------------------------+
|  rport2   | remote port 2 (data port) integer from 1 to 65535                          |
+-----------+----------------------------------------------------------------------------+
|  rport2   | remote port 2 (data port) integer from 1 to 65535                          | 
+-----------+----------------------------------------------------------------------------+

.. note::
    The odometer (odo) configuration message is written in two parts.  First an #APCFG command 
    is sent to turn it "on" or "off".  A second #APCFG command is sent to set the units. 
    On read, #APCFG returns either "off" or the selected velocity units. 