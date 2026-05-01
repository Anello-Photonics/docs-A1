Mechanicals
==================


The ANELLO Maritime INS (P/N 10001301) features a 22 pin MIL-DTL-38999 connector. The case housing is aluminum with an anodized finish.

Maritime INS SCD Drawing: :download:`PDF <media/100013R03-Outline_DrawingMaritime_INS.pdf>`


.. figure:: media/SCD_Mechanical_Maritime_INS_A0.png
   :align: center



.. note::

   If you purchased the Maritime INS Evaluation Kit, a schematic of the breakout cable can be downloaded :download:`here <media/25002231_03-25_SCD Assy Cable Maritime A0 Breakout.pdf>`.

   **Wiring Variation (Pre–February 20, 2026 Cables):**  
   Cables manufactured prior to February 20, 2026 may contain either red/black or purple/yellow CAN wiring.  
   In red/black assemblies, red corresponds to CAN_P and black to CAN_N.  
   In purple/yellow assemblies, purple corresponds to CAN_P and yellow to CAN_N.

.. note::
   The prototype Maritime INS unit (P/N 10001302) SCD drawing can be found :download:`here <media/100013,R02-Outline_Drawing,Maritime_IMU.PDF>` and a schematic of the breakout cable can be downloaded :download:`here <media/SCD_Breakout Cable_Maritime_INS.pdf>`. The pinout for the prototype connector can also be found :download:`here <media/Plug_Socket_Face.png>`

.. note::
   Total capacitance on the RESET line (cable + input + ESD, etc.) must be ≤ 10 nF; no external reset capacitor recommended.

.. note::
   The Sync, RESET, and BIT pins are high-impedance inputs and do not require 50 Ω termination.