Mechanicals
==================


The ANELLO Ground INS Gen 2 (P/N 10001301) features a 22 pin MIL-DTL-38999 connector. The case housing is aluminum with an anodized finish.

Ground INS Gen 2 SCD Drawing: :download:`PDF <media/100013R03-Outline_DrawingMaritime_INS.pdf>`


.. figure:: media/SCD_Mechanical_Maritime_INS_A0.png
   :align: center



.. note::

   If you purchased the Ground INS Gen 2 Evaluation Kit, a schematic of the breakout cable can be downloaded :download:`here <media/25-002301-22, r4_SCDAssyCableMaritimeA0Breakout.pdf>`.

   **Wiring Variation (Pre–February 20, 2026 Cables):**  
   Cables manufactured prior to February 20, 2026 may contain either red/black or purple/yellow CAN wiring.  
   In red/black assemblies, red corresponds to CAN_P and black to CAN_N.  
   In purple/yellow assemblies, purple corresponds to CAN_P and yellow to CAN_N.

.. note::
   Total capacitance on the RESET line (cable + input + ESD, etc.) must be ≤ 10 nF; no external reset capacitor recommended.

.. note::
   The Sync, RESET, and BIT pins are high-impedance inputs and do not require 50 Ω termination.