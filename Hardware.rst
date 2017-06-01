Hardware System Design
======================

..  toctree::
    :maxdepth: 2

    Hardware system design        <Hardware>

FPGA System
-----------

The system implemented in this Project is depicted in the figure below. The
biggest part of the system is a *NiosII* processor system defined in
*SOPC-Builder*. The rest of the system is defined directly in *Quartus II*.

..  image:: /_static/fpga_diagram.png
    :width: 750px
    :align: center

SOPC-Builder system
^^^^^^^^^^^^^^^^^^^

- **Nios II processor:** soft-processor controlling all the hardware components.
  It deals with the initialization of all components and the communication
  through Ethernet. Communications permit remote control of the system via
  commands. The instruction bus and data bus of the processor are connected to
  both SDRAM and Flash memory, so both this memories can be used as data and
  program memory.

- **SRAM Controller:** It controls the 2MB volatile SRAM available in the board.
  When debugging using the *Nios II EDS* it is used as program and data memory
  for the *Nios II* processor. When the development is finished and the program
  for the processor is loaded into the non-volatile flash memory, the SRAM
  memory is only the data memory for the system.

- **Flash Controller:** It controls the 8MB Flash non-volatile memory available
  in the board. When debugging using the *Nios II EDS* it can be used to store
  the MAC address of the board or any other configuration/calibration data. When
  the program is finished and the board does not need the computer anymore the
  Flash memory is used as program memory to store the program for the *Nios II*
  processor.

- **PLL:** It generates the clock signals for the system from a 50MHz source.
  *C0* and *C2* are used inside *SOPC-Builder*. *C0* to *C4* are exported
  outside *SOPC-Builder* for being used by the *Quartus II* components (orange
  part). Some of the clock signals generated in *SOPC-Builder* and exported are
  not used in *Quartus II* side because there is another PLL there. This PLL
  should be removed and the system should use the clocks generated in the main
  PLL inside Qsys.

- **Avalon Camera:** It provides a register-based interface for the
  configuration of the *Camera Controller* component from the *Nios II*
  processor.

- **APIO Sensor:** It provides a register-based interface for the configuration
  of the *Sensor Controller* component from the *Nios II* processor.

- **Clock-crossing I/O:** It connects buses with different clock signals. In
  this case it connects the secondary *Avalon MM data bus* (sourced by *C2*)
  with the main *Avalon MM data bus* (sourced by *C0*).

- **JTAG UART:** It permits to communicate *Nios II EDS*  with the Nios II, to
  use the *Nios II* console, to load the programs in the *SDRAM* and to do
  step-by-step debugging. It has an interrupt line connected to the *Nios II*
  with the highest priority 0 (lower number means higher priority).

- **Ethernet controller:** It controls the *PHYS Ethernet* chip in the board
  that implements the physical layer of the TCP/IP protocol. It can use *Direct
  Memory Access* (DMA) and read and write directly to the *SRAM* memory (data
  memory). Its interrupt has the lowest priority in the system (number 2).

- **Sys_ID:** This component stores a number identifying the system and a time
  stamp when the *Nios II* system was generated in *SOPC-Builder*. The *Nios II
  EDS* checks these two values before loading a *Nios II* program to ensure that
  the system connected is the correct target for the software project (these 2
  numbers are passed in the *sopc_info* file used to generate the software
  projects).

- **Timer:** It is the timer used by the *Real Time Operating System* (RTOS)
  running in the *Nios II* for scheduling its tasks. After the JTAG UART this
  timer should have (and has) the highest priority interrupt (number 1).

- **Tracker0 to Tracker 5:** The trackers are the components that manage the
  ROIs that will follow (track) the detected vehicles in the captured images.
  They change dynamically at every iteration, checking the movement of the
  triangle and following it. They have to be activated by receiving a command
  from the Nios II application.

- **LED PIO:** GPIO peripheral to control from *Nios II* the green LEDs 0 to 7
  in the board.

- **LCD Controller:** Used to control the LCD screen from Nios II. It is used
  by the software to print some information like the IP address.

Quartus II part
^^^^^^^^^^^^^^^

- **Camera Controller:** It is in charge of the camera configuration, RAW data
  capture from the camera and data conversion from RAW to RGB and greyscale.
  There are 4 buses of 10 bits each: one for each RGB channel and one more for
  grey scale channel.

- **Sensor Controller:** it implements the image processing for the detection of
  robots marked by red triangles, using therefore only the red channel from the
  Camera Controller. The output of the algorithm are the x and y coordinates of
  the 4 points defining the square that encloses each triangle. *X* and *Y* are
  12 bit buses and are sent to the trackers. Another output is the RGB and grey
  images that are stored to SDRAM memory through the SDRAM controller.

- **SDRAM Controller:** it controls the 128MB SDRAM memory in the board.

Besides the previous components, there are also present in the hardware design
several LEDs, which show the programmer useful information:

- Red LED 17
- *RED LED 16*, set to ON when the *Camera Controller* is providing a video
  output.
- *RED LED 15*, set to ON when the input of the *VGA controller* is connected
  to the output of of the *SDRAM controller*. If the *Camera Controller* is
  providing an image it should appear on the Screen connected to the VGA port.
- RED LED 14: set to ON when the processor orders the camera to acquire images.
- RED LED 13
- RED LED 12
- RED LED 11


Jumpers configuration
^^^^^^^^^^^^^^^^^^^^^

The physical layer of the port used for the TCP/IP communication has to be
configured in MMI Mode. This means that, as the used port is ETHERNET0, the
jumper JP1  pins 2 and 3 must be shorted. Otherwise, if pins 1 and 2 are sorted
the physical layer will be configured in RGMII Mode, and it will not be
compatible with the implemented FPGA and software projects. More information
can be obtained in the board reference manual [1]_.

As the ETHERNET1 port will not be used, the jumper defining its physical layer
(JP2) is not relevant for this project. JP3 sets the HSMC I/O on and off, and
is irrelevant for this project too, as it is used when more than one FPGA is
desired to be connected and form a closed JTAG loop chain. By default it is set
to off (pins 1 and 2 shorted).

Regarding the other jumpers, they should be kept with the default settings,
namely the JP6 to 3.3V position and the JP7 in the 2.5V position (Defining the
I/O pins voltage)


FPGA Software
-------------

As stated in the Introduction, the software project is formed by 2 subprojects:
*SocketServer*, that is the application project developed for implementing the
desired functionality; and *Socket_Server_bsp*, which defines the hardware
architecture of the system that is being used.

SocketServer
^^^^^^^^^^^^

This is the main application of the software, where the core functionalities are
implemented. It works over a hardware layout defined by the other subproject,
*Socket_Server_bsp*. The following are the main modules inside this projects:

- **trackers:** This script provides functions related to the usage of the
  trackers in the FPGA. These functions allow to read and update their state.
  The functions defined in this module are the following:

    - *trackers_init*
    - *trackers_number:* Returns the number of trackers. By default, 6
    - *trackers_free:* Sets the *active* and *id* flags of all trackers to 0.
    - *activate_tracker:* Sets the *active* and *id* flags of desired tracker
      to 1.
    - *disable_tracker:* Sets the *active* flag of desired tracker to 0.
    - *free_tracker:* Sets the *active* and *id* flags of desired tracker to 0.
    - *set_search_window_of_tracker:* sets the window of desired tracker,
      provided the x, y of the bottom left corner and its width and height
      parameters.
    - *get_search_window_of_tracker*
    - *get_current_corners_of_tracker*
    - *get_current_windows_of_activated_trackers*
    - *get_current_corners_of_activated_trackers*

- **camera:** This module contains functions for setting up the camera working
  parameters and getting their actual values, which are stored in FPGA
  registers. Once the parameters have assigned the desired values, the
  *camera_configure* function has to be called for sending them to the FPGA
  registers. In the associated header file (camera.h) it is defined a struct
  called camera-struct, containing the parameters defining the camera operation.
  The aforementioned camera parameters are the following:

    - image_size
    - image_exposure
    - start_image
    - sensor_size
    - sensor_mode
    - sensor_output
    - vga_output

- **network_utilities:** Contains functions for handling the TCP/IP protocol:

    - *get_mac_addr:* If FIXED_MAC is True, gets the default MAC address. If
      not, asks the user to input a new one.
    - *get_ip_addr:* This routine is called by InterNiche to obtain an IP
      address for the specified network adapter. It checks whether DHCP has to
      be used or not, setting the IP address to the default one if DHCP is not
      used.
    - *get_serial_number:* Asks the user the 9-digit device serial number,
      merges them all in a single variable and returns this variable.
    - *generate_and_store_mac_addr:* This function is called if any MAC address
      is not stored in the flash memory. If so,a new one is generated, based on
      the Altera vendor ID and the product serial number. Besides, the static
      IP, subnet, gateway and "Use DHCP" sections are reprogrammed. All this
      content is stored in the last sector of the flash memory.
    - *generate_mac_addr:* Similar to the previous function, it generates a
      MAC address based on the device serial number. However, it does not store
      it in the flash memory
    - *get_board_mac_addr:* Gets the MAC address stored in the *mac_addr*
      variable. Moreover, it checks if there is a MAC address stored in the
      flash memory, prints it and stores a new one if needed.
    - *FindLastFlashSectorOffset:* This function gets the value at the
      corresponding offset from the last sector in the flash memory and returns
      it in *pLastFlashSectorOffset*.

- **simple_socket_server:** This module contains functions for handling TCP/IP
  connections with a client. Thus, the program is designed to be constantly
  attending incoming petitions from the Ethernet port:

    - *sss_reset_connection:* function that sets the connection object members
      to an initial state.
    - *sss_send_menu:* Sends to the client a menu containing a summary of the
      commands.
    - *sss_exec_command:* Parses a received command from the client, and
      executes a different routine depending on the message received.
    - *sss_handle_accept:* This routine is called when ever our listening
      socket has an incoming connection request.
    - *sss_handle_receive:* Loop that will be executed since the connection
      begins until it is closed, and that continually checks if there are
      packages in the incoming buffer to be read.
    - *SSSSimpleSocketServerTask:* Thread task with an endless loop, that will
      continually check if there are requests for beginning a new connection.


- **main:** Main routine of the software project. It simultaneously runs several
  threads for dealing with different tasks:

    - *write_in_lcd:*  Writes input message in the LCD screen
    - *task1:* Shows the IP and execution time. Refreshes every second.
    - *SSSInitialTask:* Instantiates the NiosII resources, including a task for
      the TCP/IP stack (NicheStack).
    - *main:* Clears the OS timer, creates SSSInitialTask and starts the RTOS.

- **MAC_IP_config.h:** The default MAC and IP address of the board are specified
  in this header file.


Socket_Server_bsp
^^^^^^^^^^^^^^^^^

This project provides a system abstraction, as it maps the hardware developed in
the FPGA system to a common namespace, in order to make higher level code
compatible for different boards or layouts. The most important files and folders
in the BSP are:

- *Drivers*, which is a folder with the drivers of the components added in SOPC
  Builder. For custom components the drivers have to be manually added to
  drivers folder or added as a library component to Altera installation folder
  so this process is automatically performed.
- *HAL (Hardware Abstraction Layer)*, which gives support to the standard C
  library to Nios II processor.
- *UCOSII*, which are the files of the *uC-OS II* Real Time Operating System
  (RTOS). In this case this RTOS is used to schedule several tasks in the
  processor.
- *System.h*, which contains macros for addressing the *Nios II* system
  components. Using these macros is safer than using directly the addresses
  from *SOPC-Builder*, because future changes in the hardware address map can
  be accordingly fixed for the software by regenerating the BSP.



MAC and IP
^^^^^^^^^^

The used MAC and IP settings by the program are contained in the
*MAC_IP_config.h* file.

- **Using DHCP:** If USEDHCP macro is defined in the file, the board will ask
  for an IP address via DHCP using the MAC specified inside the file. If DHCP
  fails (after 2 minutes) the board sets the IP address written in the file.
- **Fixed IP:** If USEDHCP macro is NOT defined in the file (i.e. the line
  commented), the MAC and IP used will be the ones specified in the file, and
  DHCP is not used.

|

.. rubric:: **Bibliography**

.. [1] DE2-115 User Manual, 3rd edition (2014) `<http://www.terasic.com.tw/cgi-bin/page/archive_download.pl?Language=English&No=502&FID=cd9c7c1feaa2467c58c9aa4cc02131af>`_
