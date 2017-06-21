..  image:: /_static/portada.jpg
    :width: 1100px
    :align: center

Introduction
============

..  toctree::
    :maxdepth: 2

    Introduction

General  overview
-----------------

The project is aimed to control an indoor intelligent space where several
unmanned vehicles are simultaneously observed and controlled through a
distributed vision system, a central controller and an Arduino board embedded
at each vehicle. The physical system was set up in the university in order to
run tests and try new algorithms and solutions.

Hence, the whole system can be divided into three different elements:

- The *main controller*, which consists in a CPU e.g. a PC or an embedded
  SoC (ZedBoard) that controls the whole system. The *uvispace* software project
  is executed there, whose main tasks are:

    - Communicate with the FPGA-based localization nodes, using the Ethernet
    LAN network.
    - Merge the data obtained from the localization nodes.
    - Get the global coordinates of the UGVs.
    - Given the destination of the UGVs, calculate the optimal path.
    - Calculate the UGVs speed, using a navigation model.
    - Communicate with the Arduino boards, using the XBee protocol, and
      send them the speed set points.

- The 4 *localization nodes*. Their main component is an FPGA, with a camera
  peripheral. Each camera frame is processed in the FPGA, and the obtained
  results are sent to the *main controller* through the Ethernet LAN network.

- The *Arduino boards*, that control each UGV's sensors and actuators. They
  receive orders from the *main controller* through the data received
  from the serial port, connected to a XBee transceiver.

..  image:: /_static/uvispace_elements.png
    :width: 750px
    :align: center

The main structure of the system can be observed in the diagram below, where the
communications between the three different elements aforementioned are
represented. The system communications master is the *main controller*,
which controls the *Arduino boards* using an IEEE 802.15.4-based protocol,
namely a variation of the ZigBee specification. It controls as well the *image
localization nodes* using the Internet protocol through the Ethernet network.

..  image:: /_static/general_diagram.png
    :width: 750px
    :align: center

Regarding the control workflow, the system can be considered a closed loop,
where the position of the UGV read by the cameras is fed back to the main
controller that, along with the goal position, are used to determine the
speed output sent to the UGV. The corresponding diagram can be seen below.

..  image:: /_static/workflow_control_diagram.png
    :align: center

Software project
----------------

The software subproject inside the UviSpace project is called uvispace as well.
It is a **Python2.7** project, and it is structured into 2 main packages, namely
*uvirobot* and *uvisensor*.

The *uvirobot* package deals with the classes and functions needed to implement
the algorithms for moving an UGV and communicating with it, while the
*uvisensor* package is used to communicate with FPGA devices using a TCP/IP
connection and receive scene information captured through camera peripherals.

uvirobot
^^^^^^^^

The *uvirobot* package consist in 2 callable scripts:

- **messenger.py** establishes the communication to the specified UGV, using the
  XBee protocol. Thus, prior running it an XBee module has to be connected to
  the PC, and another one to the Arduino board serial port, and both configured
  accordingly. Once the communication is established, the module *listens* for
  speed set points and send them to the UGV. When the execution is canceled,
  a plot with the delay times is obtained and then the program ends.
- **controller.py** *listens* for input UGV position values and destination
  coordinates, calculates the UGV optimal path, and finally outputs the speed
  set points needed to achieve the planned path.

Moreover, there are 5 importable libraries inside the *uvirobot* package:

- **path_tracker.py** contains a class whose methods calculate an UGV's path
  points, once given a position and destination, and then stores them in an
  attribute.
- **plotter.py** contains functions used to construct a graph with a predefined
  format. It is designed to map the calculated vs. real path of a UGV, and the
  delay times.
- **robot.py** contains the *RobotController()* class, where each instantiated
  object represents a UGV, and establishes the upper interface for working
  with its speed values.
- **serialcomm.py** contains the *SerMesProtocol()* class, which is a child of
  the built-in python *pyserial* class. It defines a serial protocol, which will
  be used by the XBee modules for communicating the CPU with a UGV.
- **speedtransform.py** contains the *Speed()* class, for dealing with
  operations related to the speed values, such as transform between different
  scales or ensuring that the values are between valid boundaries.

Finally, the package contains a couple of auxiliary modules into the *resources*
folder:

- *teleoperation.py* is a callable module that allows to control the UGV with
  the keyboard, which is specially useful when the *uvispace* package has to be
  tested.
- *set_goal.py* is a callable module that allows setting new goal points when
  the system is running.

uvisensor
^^^^^^^^^

The *uvisensor* contains 1 callable module, **multiplecamera.py**. It is based
on multithreading. The purpose of this module is to manage the information of
N cameras, merge the vertices of every tracker and obtain the final number of
UGVs(triangles) with their pose. There is a shared variable with all the
vertices obtained from each camera.

The connection via TCP/IP to the cameras, as well as the processing of the
images from each camera, is achieved in parallel.

Finally, another thread communicates with the user in order to get commands. At
the moment, the only command is 'Quit Program' ('Q'), but this functionality can
be easily increased in the future.

Summing up, the list of threads is:

* 1 main thread that will merge the information from every camera (VideoSensor).
* N threads for getting data from the N cameras connected to the system.
* 1 user oriented thread for getting commands from the user.

The package has, as well, 4 importable modules:

* **client.py** contains the *Client()* class, which is a child of the *Socket*
  class from the *socket.socket* built-in Python module. This class contains
  methods for communicating specifically with the design FPGA hardware. Its
  methods allow to open and close the connection correctly, and to write and
  read from valid registers with the right format.
* **geometry.py** contains 2 classes. The *Triangle()* class is used for
  performing geometrical operations inherent to isosceles triangles, in order
  to get its base length, barycentre, position, angle... The *Segment()* class
  is used to determine a segment from its 2 points, and calculate afterwards
  the distance to another point.
* **imgprocessing.py** contains the *Image()* class, which has image-oriented
  methods, based on matrix operations, for getting useful information from
  image data.
* **videosensor.py** has the *VideoSensor()* class and several functions related
  to it. Each instantiated object represent an external FPGA device. Thus, this
  class has methods for setting up the TCP/IP connection, configuring the FPGA
  registers, and interact with it.

..  image:: /_static/software_diagram.png
    :width: 750px
    :align: center

Hardware design project (DEPRECATED)
-------------------------------------

As stated before, the hardware system was designed for being implemented on an
FPGA-based board and its main purpose is to provide a hardware interface for
reading the raw data from the cameras and processing it, sending afterwards
information regarding the captured scene to the *Data Fusion Controller*.

This design can be found on the *FPGA* subfolder, and it namely consists on 2
different projects: *DE2_115_sopcbuilder_12* and
*DE2_115-flash_sopcbuilder_11_original*. The first one is for debugging, and
can only be loaded from the PC on the SRAM memory in the board, being lost after
shutdown; whereas the second one can only be loaded and stored on the flash
memory inside the board, and it will not be lost after shutdown.

Physical layout
^^^^^^^^^^^^^^^

The main component of the vision system is a **Terasic DE2-115** board, which
contains an *Altera Cyclone IV FPGA*. With 115k logic elements, this is a fairly
big FPGA, suitable for complex digital projects. Connected to the main board,
through the 40-pin GPIO , there is a *Terasic D5M* 5 Megapixel camera.

In this project the 40-pin straight female header in the camera is changed by a
90 degree 40-pin header so the camera points down. 4 of these cameras are
located in the ceiling of the laboratory like the following figure shows. The
cameras are located so the field of view of the cameras touch with each other
without overlapping. This is done this way to maximize the area the cameras can
see. Problems to locate the car in the borders of the cameras should be solved
by software.

..  image:: /_static/terasic_DE2-115.png
    :width: 500px
    :align: center

FPGA hardware
^^^^^^^^^^^^^

The biggest part of the system implemented in this project is a *NiosII*
processor system defined in *SOPC-Builder*. The rest of the system is defined
directly in Quartus II.

FPGA software project
^^^^^^^^^^^^^^^^^^^^^

The software project that runs in the Nios II processor is basically composed
by two subprojects:

- **SocketServer**, which is an application project containing the
  application-specific code.

- **Socket_Server_bsp**, which contains all the code that defines the interface
  with the hardware, creating an abstraction layer which eases the understanding,
  development and migration of code by a user.

Arduino controllers project
---------------------------

The actuators and sensors of the UGVs are managed by an Arduino board on each
of them. Due to the need to control 2 DC-motors, it is an essential requirement
of the boards to have a DC motor driver, or include instead an external one.
The developed Arduino project was implemented on the laboratory using a board
accomplishing the first option, namely the `Arduino Romeo
<https://www.dfrobot.com/index.php?route=product/product&product_id=656#.V5iRYe2Y5hE>`_
board.

The work load on the boards is intended to be minimum, and thus obtain the data
from the *Data Fusion Controller* as processed as possible. The software on them
is built from an *Arduino* project, and is divided into 3 different parts:

* **BoardParams.h** is a header containing the PIN mapping of the board and
  constants relating to the implemented serial protocol (Commands, functions and
  master/slave identifiers).
* **UGV.ino** contains the *setup* function and the main loop of the program,
  which is an infinite loop that continually checks incoming messages from the
  master. The work pipeline when a message is received is the following:

    * Check that the *STX* initial byte is correct (`byte[0]`).
    * Read the following auxiliary bytes (`byte[1..5]`) and store in the
      corresponding variables (*id_slave, id_master, fun_code* and *length*).
    * Read the number of bytes indicated in *length* variable, and store on
      *data* variable.
    * Check that the *ETX* end byte is correct (`byte[length+6]`).
    * Call *process_message* function.

* **Functions.ino** contains auxiliary functions that are run in different is
  situations and always after a message is received and read by the main loop.
  The aforementioned functions are the following:

    * *process_message* is the function called after a message is read, and
      deals with the data received. depending on the value stored in *fun_code*,
      different routines are run. In any case, at the end of the function, an
      *ACK_MSG* message is sent back to the master (unless an invalid *fun_code*
      is received).
    * *move_robot* gets the speed set point for each wheel motor of the UGV and
      applies it to the corresponding output PINs. Prior to write on them, the
      values are clipped if they are out of valid limits. It is also checked if
      they belong to a *direct* or *reverse* direction, and writes accordingly
      to the direction PINs
    * *publish_data* is called at the end of the *process_message* function,
      and deals with the construction of a valid answer message, according to
      the serial protocol. Afterwards, it is *'printed'* to the serial port, in
      order to be sent back the answer message to the master.

About us
--------

The project was developed by a team of researchers at the *Electronic Technology
Department* in the `University of Vigo <http://uvigo.gal/uvigo_en/index.html>`_.
