=========================
Troubleshooting
=========================

Software Tools
----------------------------

AMarinerControl
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^
On Ubuntu/Debian, AMarinerControl AppImage may require:

.. code-block:: bash
    :caption: Terminal

    	sudo apt update
		sudo apt install libpulse0 libxcb-cursor0 libxcb-shape0


MacOS
^^^^^^^^^^^^^^^^^^^^^^^^^^^

AMarinerControl for MacOS is unsigned so you may receive the following error message when installing for the first time.

.. image:: media/Mac_error.png
   :width: 70%
   :align: center

To open the app, go to System Settings > Privacy and Security, scroll to the bottom of the page and select "Open Anyways" to the dialogue "'AMarinerControl' was blocked to protect your Mac"

.. image:: media/Mac_open1.png
   :width: 70%
   :align: center

.. image:: media/Mac_open2.png
   :width: 70%
   :align: center

When prompted, enter your MacOS password and proceed with opening the app.

.. image:: media/Mac_pw.png
   :width: 70%
   :align: center

ANELLO INS Scripts
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

anello_fw_uploader.py
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Firmware updates requires the installation of the python libraries pyserial and pymavlink

.. code-block:: bash
    :caption: Terminal

    	pip3 install pyserial pymavlink