Configurations
================

This page consolidates the Maritime INS parameter tables used throughout the documentation.
Use the links below to jump directly to a configuration group.

* :ref:`installation-parameters`
* :ref:`nmea-2000-parameters`
* :ref:`ethernet-parameters`
* :ref:`nmea0183-over-udp-parameters`
* :ref:`can-termination`

.. _installation-parameters:

Installation Parameters
-----------------------

The lever arms of the installation must be measured and configured as parameters to ensure accuracy.
The coordinate system follows the right-hand rule: **X = forward**, **Y = right**, **Z = down**.
The INS center is the center of the Maritime INS unit.

Distances are measured in meters from the IMU center to the respective antenna phase center.

+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| Parameter           | Units | Default | Description                                                                                  |
+=====================+=======+=========+==============================================================================================+
| **GPS_SEP_BASE_X**  | m     | 0       | X offset from INS center to Base antenna (ANT1).                                             |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **GPS_SEP_BASE_Y**  | m     | 0       | Y offset from INS center to Base antenna (ANT1).                                             |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **GPS_SEP_BASE_Z**  | m     | 0       | Z offset from INS center to Base antenna (ANT1).                                             |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **GPS_SEP_ROVER_X** | m     | 0       | X offset from INS center to Rover antenna (ANT2).                                            |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **GPS_SEP_ROVER_Y** | m     | 0       | Y offset from INS center to Rover antenna (ANT2).                                            |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **GPS_SEP_ROVER_Z** | m     | 0       | Z offset from INS center to Rover antenna (ANT2).                                            |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **EKF2_IMU_POS_X**  | m     | 0       | X offset from center of boat to INS center.                                                  |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **EKF2_IMU_POS_Y**  | m     | 0       | Y offset from center of boat to INS center.                                                  |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **EKF2_IMU_POS_Z**  | m     | 0       | Z offset from center of boat to INS center.                                                  |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **SENS_BOARD_ROT**  | enum  | 0       | INS mounting orientation. Set this if unit is not mounted with X-forward.                    |
|                     |       |         |                                                                                              |
|                     |       |         | *Common values:*                                                                             |
|                     |       |         |   - **No Rotation**: Unit mounted upright with X pointing towards vessel bow                 |
|                     |       |         |   - **Yaw 90°**: Unit mounted upright with X pointing towards vessel starboard               |
|                     |       |         |   - **Yaw 180°**: Unit mounted upright with X pointing towards vessel stern                  |
|                     |       |         |   - **Yaw 270°**: Unit mounted upright with X pointing towards vessel port                   |
|                     |       |         |   - **Roll 180°**: Unit mounted upside down with X pointing towards vessel bow               |
|                     |       |         |   - **Roll 180°, Yaw 90°**: Unit mounted upside down with X pointing towards vessel starboard|
|                     |       |         |   - **Roll 180°, Yaw 180°**: Unit mounted upside down with X pointing towards vessel stern   |
|                     |       |         |   - **Roll 180°, Yaw 270°**: Unit mounted upside down with X pointing towards vessel port    |
|                     |       |         |                                                                                              |
|                     |       |         | Will be presented as a drop-down menu in AMarinerControl.                                    |
+---------------------+-------+---------+----------------------------------------------------------------------------------------------+

.. _nmea-2000-parameters:

NMEA 2000 Parameters
--------------------

To enable the NMEA 2000 driver, ensure the parameter ``NM2K_CFG`` is set to 1. Then, each published PGN has an associated output data rate parameter in the
**NM2K** group (e.g. ``NM2K_129025_RATE``, ``NM2K_129026_RATE``,
``NM2K_129029_RATE``). Rates are specified in Hertz and are clamped between
``0`` and ``100``. Setting a value to ``0`` stops transmission of that PGN; any
positive value defines the broadcast frequency. Update the rates from
AMarinerControl's parameter editor or from the command-line interface.

+------------------+-----------+--------------------------------------------------------------------------+
| Parameters       | Default   | Description                                                              |
+==================+===========+==========================================================================+
| CAN_TERM         | 0         | Enable or disable 120ohm CAN termination. 0 is disable, 1 is enable      |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_126992_RATE | 0         | Message rate for the PGN specified (Time data)                           |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_127250_RATE | 0         | Message rate for the PGN specified (Heading data)                        |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_127251_RATE | 0         | Message rate for the PGN specified (Rate of turn data)                   |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_127257_RATE | 10        | Message rate for the PGN specified (Attitude data)                       |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_129025_RATE | 0         | Message rate for the PGN specified (Latitude and longitude data)         |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_129026_RATE | 0         | Message rate for the PGN specified (Course over ground data)             |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_129029_RATE | 0         | Message rate for the PGN specified (Position data)                       |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_BITRATE     | 250 kbps  | CAN Bitrate                                                              |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_CFG         | 1         | Enable or disable NMEA2000 driver. 0 is disable, 1 is enable             |
+------------------+-----------+--------------------------------------------------------------------------+
| NM2K_SRC_ADDR    | 254       | Populates the source address field in the NMEA2000 frame                 |
+------------------+-----------+--------------------------------------------------------------------------+

.. _ethernet-parameters:

Ethernet Parameters
-------------------

Ethernet settings can be configured using the following parameters:

+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| Parameter           | Default (human readable) | Default (int32)    | Description                                                                             |
+=====================+==========================+====================+=========================================================================================+
| **NET_CFG_PROTO**   | DEVICE=eth0              | 1                  | Network device interface name.                                                          |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| **NET_CFG_NETMASK** | NETMASK=255.255.255.0    | -256               | Network subnet mask.                                                                    |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| **NET_CFG_IPADDR**  | IPADDR=192.168.0.3       | -1062731773        | Static IP address assigned to the interface.                                            |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| **NET_CFG_ROUTER**  | ROUTER=192.168.0.254     | -1062731522        | Default gateway (router) for the network.                                               |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| **NET_CFG_DNS**     | DNS=192.168.0.254        | -1062731522        | DNS server address.                                                                     |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| MAV_2_UDP_PRT       | 14550                    | 14550              | MAVLink UDP port number (INS side)                                                      |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+
| MAV_2_REMOTE_PRT    | 14550                    | 14550              | MAVLink UDP remote port number (PC side)                                                |
+---------------------+--------------------------+--------------------+-----------------------------------------------------------------------------------------+

Ethernet IPv4 Parameter Encoding
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ethernet configuration parameters (IP address, netmask, router, DNS) are stored
internally as **signed 32-bit integers (int32)**, not as human-readable
IPv4 strings.

The following Python helper function converts a standard IPv4 string
(e.g. ``"192.168.0.3"``) into the signed int32 value used by the Ethernet
configuration parameters.

.. code-block:: python

    # ==================================================================================
    # IPv4 string → signed int32 conversion
    # ==================================================================================
    def ipv4_to_int32(ip_str):
        """
        Convert readable IPv4 like '192.168.0.2' into signed 32-bit integer
        """
        parts = ip_str.split('.')
        if len(parts) != 4:
            raise ValueError("Invalid IPv4 address format: %s" % ip_str)

        a, b, c, d = [int(p) for p in parts]

        unsigned32 = (a << 24) | (b << 16) | (c << 8) | d

        # Convert to signed 32-bit
        if unsigned32 >= (1 << 31):
            signed32 = unsigned32 - (1 << 32)
        else:
            signed32 = unsigned32

        return signed32

The same logic is already implemented in the ANELLO INS Scripts repository:
`Maritime_INS_CFG.py (ANELLO INS Scripts) <https://github.com/Anello-Photonics/ANELLO_INS_Scripts/blob/main/Maritime_INS_CFG.py>`_

Port number configs can be changed directly in AMC.

.. _nmea0183-over-udp-parameters:

NMEA0183 over UDP Parameters
----------------------------

If utilizing NMEA0183 messaging over UDP, the multicast IP for NMEA0183 UDP messaging can be set with the following 4 parameters:

+-------------------+-------+---------+----------------------------------------------------------------------------------------------+
| Parameter         | Units | Default | Description                                                                                  |
+===================+=======+=========+==============================================================================================+
| **NMUDP_MC_IP0**  | byte  | 0       | Multicast IPv4 address octet 0 (most significant byte). Forms the first field of the         |
|                   |       |         | dotted-decimal multicast address (e.g. ``224.x.x.x``).                                       |
+-------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **NMUDP_MC_IP1**  | byte  | 0       | Multicast IPv4 address octet 1. Forms the second field of the dotted-decimal multicast       |
|                   |       |         | address (e.g. ``224.1.x.x``).                                                                |
+-------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **NMUDP_MC_IP2**  | byte  | 0       | Multicast IPv4 address octet 2. Forms the third field of the dotted-decimal multicast        |
|                   |       |         | address (e.g. ``224.1.1.x``).                                                                |
+-------------------+-------+---------+----------------------------------------------------------------------------------------------+
| **NMUDP_MC_IP3**  | byte  | 0       | Multicast IPv4 address octet 3 (least significant byte). Forms the fourth field of the       |
|                   |       |         | dotted-decimal multicast address (e.g. ``224.1.1.1``).                                       |
+-------------------+-------+---------+----------------------------------------------------------------------------------------------+

The default output port is 19550 and input port is 19551.

.. _can-termination:

CAN Termination
---------------

The ANELLO Maritime INS supports configurable internal CAN termination.

+------------------+---------+-----------------------------------------------------------+
| Parameter        | Default | Description                                               |
+==================+=========+===========================================================+
| **CAN_TERM**     | 1       | CAN bus termination setting.                              |
|                  |         | **0** = No termination resistor.                          |
|                  |         | **1** = 120 Ω termination resistor enabled.               |
+------------------+---------+-----------------------------------------------------------+

.. note:: Configurable CAN termination supported for production units (P/N 10001301), not for evaluation units (P/N 10001302)
