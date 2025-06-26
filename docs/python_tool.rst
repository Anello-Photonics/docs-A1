ANELLO Python Tool
=====================

The ANELLO Python Program can be used to connect, configure, and log data with any ANELLO product. 
It works on any computer running Windows, macOS, or Linux (including Ubuntu).
It can also be used as an NTRIP client to forward RTK corrections to your EVK or GNSS INS for enhanced GPS accuracy.
Please see the instructions below to use the ANELLO Python program and contact support@anellophotonics.com with any questions. 

Install ANELLO Python Program
-------------------------------------

Confirm that `python <https://www.python.org/downloads/>`_ is installed on your computer and the version is at least 3.6.
Open a terminal and type:

.. code-block:: python
    :caption: Terminal

    python --version

.. note::
    If the above shows python 2.x despite Python 3 being installed, try "python3 --version". 
    If that shows python 3.x, please use "python3" instead of "python" when running user_program.py from the command line.

Make sure you have a `git client <https://git-scm.com/download>`_ on your computer as this is the easiest way to stay up-to-date with the ANELLO Python Program.

Clone the GitHub repository:

.. code-block:: python
    :caption: Terminal

    git clone https://github.com/Anello-Photonics/user_tool.git

.. note::
    Please run "git pull" regularly to make sure you are using the latest Python tool features.

Install dependencies using pip:

.. code-block:: python
    :caption: Terminal

    cd user_tool
    pip install -r requirements.txt

If you have any issues, see `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/imu_plus/setup_troubleshooting.html#install-anello-python-program>`__.

Run the Python Tool 
-------------------------------------

.. code-block:: python
    :caption: Terminal

    python user_program.py

You will see *System Status* at the top, and *Main Menu* below. For more information, see `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/imu_plus/setup_troubleshooting.html#run-python-program>`__.

Connect to ANELLO Unit
-------------------------------------

Connect Over Serial
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Ensure the power cable is connected and the green power light is illuminated. Connect the ANELLO unit to the computer using the USB-C interface.

Use the arrow keys to select *Connect*, then *COM*, then *Auto* to auto-detect the unit. You can also use *Manual* if you know the data and config ports.
You should now see the *System Status* updated with the device information.

For more information or if you experience any errors, see the `Set-Up Troubleshooting <https://docs-a1.readthedocs.io/en/imu_plus/setup_troubleshooting.html#connect-to-anello-unit>`__.


Set ANELLO Configurations
-------------------------------------

Unit Configurations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
In main menu, select *Unit Configuration* to see default configurations. To change any configurations, 
select *Edit*, then the configuration to change, then select the new value.

Please see `Unit Configurations <https://docs-a1.readthedocs.io/en/imu_plus/unit_configuration.html>`_ for more information on available configurations.


Data Collection
---------------------------------

In the main menu, select *Log*, then *Start*. Use the default filename or enter a custom name. 
The *System Status* will be updated with the logging information.

To end a log, select *Log* then *Stop*. Log files are saved in the "logs" directory in user_tool, grouped by month and day.

To export a log to CSV, Select *Log*, then *Export to CSV*, then choose the log file.
CSV files for each message will be saved in the "exports" directory, under the name of the original log file. 
For more information on the output messages, see `Comminication & Messaging <https://docs-a1.readthedocs.io/en/imu_plus/communication_messaging.html>`_.


Monitor Output
-------------------------------------
For a real-time display of the ANELLO data, select *Monitor* in the main menu.

Logging can be started/ended by clicking the LOG button.
If the LOG button is red, that means data is not logging.

