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
|  gps1     |  use the first gps antenna: "on", "off"                                    |
+-----------+----------------------------------------------------------------------------+
|  gps2     |  use the second gps antenna: "on", "off"                                   |
+-----------+----------------------------------------------------------------------------+
|  odo      |  odometer "on", "off", "mps", "mph", "kph", "fps"  (See Note)              |
+-----------+----------------------------------------------------------------------------+
|  dhcp     |  DHCP "on", "off" : automatically assign A-1 IP                            |
+-----------+----------------------------------------------------------------------------+
|  lip      |  A-1 IP (aaa.bbb.ccc.ddd) for UDP                                          |
+-----------+----------------------------------------------------------------------------+
|  rip      |  connected computer IP (aaa.bbb.ccc.ddd) for UDP                           |
+-----------+----------------------------------------------------------------------------+
|  rport1   |  UDP computer port 1 (data port): integer from 1 to 65535                  |
+-----------+----------------------------------------------------------------------------+
|  rport2   |  UDP computer port 2 (data port): integer from 1 to 65535                  |
+-----------+----------------------------------------------------------------------------+

.. note::
    The odometer (odo) configuration message is written in two parts.  First an #APCFG command 
    is sent to turn it "on" or "off".  A second #APCFG command is sent to set the units. 
    On read, #APCFG returns either "off" or the selected velocity units. 