Set-Up Troubleshooting
======================

1   Install Anello Python Program
-----------------------------------
Confirm that Python is installed and the version is at least 3.6.0:

.. code-block:: python
    
    >python -V

.. note::
    If "python -V" shows version 2 despite Python 3 being installed, try "python3 -V". If that shows at Python 3.x, use "python3" instead of "python" in the following steps from command line.

In order to most easily upgrade the Anello Python Program, directly cloning from the 
the GitHub repository is recommended.  

.. code-block:: python

    git clone https://github.com/Anello-Photonics/user_tool.git

.. note::
    If you do not have a git client installed, you can (a) download/install a git client from 
    `<https://git-scm.com/download>`_ or (b) download and unzip the source code as shown in image below.

.. figure:: media/git_download.png
   :align: center
   
   Figure 3: Zip File Direct Download (Use of *git clone* is preferred method)

Install dependencies using pip:

If this is your first time to run user_program.py, you may need to install dependencies with PIP.
PIP is Python's package manager, and it is usually installed by default in Python installations.  If you are unfamiliar
with PIP a quick start guide is found here `<https://pip.pypa.io/en/stable/quickstart/>`_

.. code-block:: python

    cd user_tools
    pip install -r requirements.txt

If this fails, you may need to replace "pip" with "pip3", "python -m pip" or "python3 -m pip"

On some Linux systems, matplotlib and numpy dependencies must be installed with apt instead of pip.
Instead of using requirements.txt, do:

.. code-block:: python

    pip install cutie
    pip install pyserial
    pip install PySimpleGUI
    sudo apt install matplotlib
    sudo apt install numpy

Or if you already installed requirements.txt and ran the program, but had a matplotlib or numpy error:

.. code-block:: python

    pip uninstall matplotlib
    pip uninstall numpy
    sudo apt install matplotlib
    sudo apt install numpy

2   Connect to EVK
--------------------
Ensure the Power Cable is connected and the Green power light is illuminated.  To get started and 
perform initial unit configuration, use the USB-C interface.  Connect the USB-C cable between your computer 
and the Anello A-1.  From the board_tools directory, run user_program.py. 

.. code-block:: python

    cd board_tools
    python user_program.py

Then the interface should show in the terminal, as in figure 4 below.
This program uses a keyboard interface. Move the cursor up and down with arrow keys and select with enter key. For some settings you will enter text.

.. note::
    On some Windows computers, the arrow keys did not move the cursor.
    This appears to be an issue with the readchar dependency, version 3.0.5 on Windows.
    It can be fixed with this command in terminal:

    pip install readchar==3.0.4


The Anello Python Program is divided into two subsections as shown in the image below.  The System Status 
and a Main Menu.   The A-1 unit will shows as **not connected**, until the A-1 is explicitly connected via the
Connection option.      

.. figure:: media/full_status_labeled.png
   :scale: 50 %
   :align: center

   Figure 4: Anello Python Program Home Screen

The main menu actions are:

-   Refresh:    Refresh the display to see new system status.
-   Connect:    Connect the app to the A-1 over com or udp to configure and log
-   Configure:  edit A-1 device configurations such as udp connection settings, output data rate
-   Log:        collect the A-1 raw message output into a file. Can convert to CSV.
-   Monitor:    opens a display showing the latest INS message contents.
-   Ntrip:      connect to a server for navigation corrections.
-   Upgrade:    upgrade the A-1 with a newer firmware version
-   Exit:       exit the program


3   Connect to the EVK
----------------------------
Select the Connect option form the selection menu and press return. Select COM and then Auto. The unit will
be auto detected via Serial over USB-C.  

The Anello A-1 uses two logical ports: 

    +-------------------------+-----------------------------------+
    | **Logical Port**        |  **Physical Port** (Serial/USB-C) |
    +-------------------------+-----------------------------------+
    |  Data Port              | lowest port number e.g., COM7     |
    +-------------------------+-----------------------------------+
    |  Configuration  Port    | highest port number e.g., COM10   |
    +-------------------------+-----------------------------------+
     

Once connected the System status should be updated and the mapping of the logical ports to the virtual com 
ports shows in the System Status. When using UDP, the user has the flexibility to assign the data port and user
messaging port through the Anello Python Program.

If the auto detection fails, you can try manual connection.  First check that there are four virtual com ports. 
On Windows, use the device manager to find the COM ports.  On MAC and Ubuntu, use the terminal and change directory to */dev*, 
and check for four consecutive ports, typically named something like *tty.usbserial-xxx* on MAC/Ubuntu.

.. note::
    Anello A-1 generates four virtual com ports on the host; however only two are used. The numerically 
    highest port is the configuration/control port.  The numerically lowest port is the data port. 
    Communication occurs at a fixed baudrate of 921600 bits per second.

.. note::
    If the four COM ports do not show in the manual connection mode or Windows device manager, you may need to install the FTDI drivers from https://ftdichip.com/drivers/d2xx-drivers/

On Ubuntu or other operating systems, the program may not have permission to access serial ports causing the connect step to fail.
This can be fixed by increasing user permissions or running as root.

1. Increasing user permissions:
The user may need to be added to groups "tty" or "dialout" to access the serial port.

.. code-block:: python

    sudo usermod -a -G tty <your user name>
    sudo usermod -a -G dialout <your user name>

Then log out and back in for the permissions to apply.

2. Running as root:
Root may have a different default python, so check your python location with :

.. code-block:: python

    which python

then run as root using that path to python:

.. code-block:: python

    sudo <path to python> user_program.py

