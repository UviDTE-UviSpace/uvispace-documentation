..  image:: /_static/introduction-figs/cover2.jpg
    :width: 1100px
    :align: center

Introduction
============

..  toctree::
    :maxdepth: 2

    Introduction            <introduction>

General overview
-----------------

The project is aimed to control an indoor intelligent space where several
Unmanned Ground Vehicles (UGVs) are simultaneously observed and controlled
through a distributed vision system. The following figure shows the current
setup in the University of Vigo where the different parts of the system can be
identified.

..  image:: /_static/introduction-figs/uvispace-elements.png
    :width: 750px
    :align: center

The parts of the vehicle control system already implemented are:

- *Localization system*: It is composed by four camera-based localization nodes,
  each of which is made of a CMOS camera and an FPSoC development board.
  FPSoC contain powerful processor and FPGA in the same chip. In the current
  implementation the FPGA acquires images from the camera and preprocesses them
  binarizing the UGV markers (geometrical shapes placed over the UGV top plate).
  The binarized images are written to the processor which uses high level
  Python functions to extract the vertices of the marker. These vertices
  are sent to the main controller through the Ethernet LAN network. Apart
  from the vertices the nodes send the RGB, gray and binary images from the
  camera, useful for debugging purposes.

- The *main controller* consists of a CPU-based processing node that runs
  Python 3 software. Currently a PC is being used but the project is designed
  to be lightweight enough using common libraries, and is prepared to be run
  in any embedded board like  Raspberry Pi or (ZedBoard) that controls the whole
  system. The main controller is currently intended to be run in console.
  Currently only one vehicle can be controlled. The main controller does:

    - Communicate with the FPGA-based localization nodes, using the Ethernet
      LAN network, and asks the vertices of the markers in the field of view
      of the camera.
    - It merges the data obtained from the localization nodes. In example,
      when an UGV is in the frontier between two cameras there are marker
      vertices from the same UGV in different cameras.
    - Get the pose (position and orientation) of each UGV from the vertices location.
    - Given the destination of the UGVs, calculate the optimal path. Now an array
      of points that the UGV should visit are passed through the console.
    - Calculate the UGVs motor speed, using a navigation model, to pass
      through the set of points previously added via console.
    - Communicate with the Arduino boards in the UGVs, using the Zigbee protocol
      or WiFi, and send them the speed set points of the motors.
    - The software also reads the battery level of the UGV and prints it in console.

  Apart from the main behaviour the main controller has software to test
  different software like the Kalman Filter developed or the controller. There
  is also software to acquire images and video from the camera nodes.

- The *UGVs*. The UGVs are controlled by an Arduino board that receives
  the motor speed workpoints thorugh a Wifi-to-serial or Zigbee-to-serial
  Arduino shield and sets the PWM of the motors to the corresponding values.
  It also reads the battery level from a fuel gauge board if it is installed
  in the vehicle.

The main structure of the system can be observed in the diagram below:

..  image:: /_static/introduction-figs/general-diagram.png
    :width: 750px
    :align: center

Regarding the control workflow, the system can be considered a closed loop,
where the position of the UGV read by the cameras is fed back to the main
navigator that, along with the goal position, are used to determine the
speed output sent to the UGV. The corresponding diagram can be seen below.

..  image:: /_static/introduction-figs/workflow-control-diagram.png
    :align: center

Currently the development of the first UGV with differential
configuration has been completed. A set of three Printed
Circuit Boards (PCBs) has been developed to monitor the health and state of
charge of the UGV battery and safely charge it using an external power supply.
A Wireless Power Transfer (WPT) system has also been developed to charge the
battery without human intervention. A basic version of the main controller
already allows the UGVs to be moved along trajectories consisting of straight
segments.


Localization system
---------------------

The localization system is one of the most innovative parts of Uvispace.
It is composed by a Terasic TRDB-D5M 5MPix camera attached by a 40-pin
connector to a FPSoC board. FPSoC are novel chips that contain a powerful
processor and FPGA in the same chip. They permit hardware advantages
(performance and parallelization) and software advantages (ease to develop and port)
to be combined in a very efficient way. The FPSoC boards currently supported
for the localization system are Terasic DE1-SoC and DE0-nano-SoC, both containing
the Intel FPGA Cyclone V SoC chip. The following figure shows how the camera
and the boards are connected:

..  image:: /_static/introduction-figs/boards-pic.png
    :width: 650px
    :align: center

 In the final implementation the cameras are placed over platforms hanging from
 the labs ceiling and are bended 90 degrees, so they point to the ground (and UGVs),
 like shown in the following figure:

..  image:: /_static/introduction-figs/boards-ceiling-lab.png
     :width: 650px
     :align: center

The current figure shows a simplified block diagram of the system.

..  image:: /_static/introduction-figs/loc-node-diagram.png
    :width: 650px
    :align: center

The FPGA is in charge of image capture and preprocessing. RGB images are acquired
from the camera and binarized. In binary images, the marker in the
top of the UGV is clearly differentiated from the background, thus enabling
localization. Erosion and dilation are applied to binary images to remove noise
while keeping triangle size unchanged. Finally, the preprocessed image is stored
by the FPGA in SDRAM memory shared with the HPS. Rough estimates show that around
300 fps might be achieved for 640x480 images when implementing preprocessing in
the FPGA (but frame rate is limited by the maximum throughput of the camera, 150fps)
whereas only 15 fps are achieved when using the processor. This clearly
justifies the FPGA implementation of image preprocessing. The following
figure shows how the FPGA preprocessing transforms the image from RGB to
binary when using triangular red markers:

..  image:: /_static/introduction-figs/rgb-and-binary.png
    :width: 750px
    :align: center

The processor reads the binary image from shared SDRAM memory, searches it for
triangles and identifies their vertices, whose (x, y) pixel coordinates
are sent to the main controller via Ethernet. With this information, following
the procedure described in [1]_ the main controller determines the pose of each
UGV: (x, y) coordinates of its center (mm) and orientation angle θ (degrees).

As it can be observed this architecture is very convenient
for this implementation because the image processing can be done
very efficiently in the FPGA while the higher level tasks (vertices identification
and communications) are more easily implemented by software in the processor.


UGVs
----

The UGVs currently used in UviSpace are differential robots from DF robot.
The models currently being used are the `Baron-4WD <https://www.dfrobot.com/product-261.html>`_
and variants with the same motors.

..  image:: /_static/introduction-figs/pirate-4D.jpg
    :width: 750px
    :align: center

These chasis have four motors but they are controlled by 2 PWM signals. That is,
the motors in the left are controlled by a single signal and the motors in the
right are controlled by another signal. In that sense they are equivalent to a
differential robot. The chasis from DF Robot are completed with the following
elements:

- Battery: A 2 Cell LiPo battery with 220mAh of capacity and 7.4V of nominal
  voltage is used to power up the UGV. The battery should be charged through a
  charger board or a  universal charger for this kinds of batteries like the
  `HOBBYMATE Imax B6 Clone Lipo Battery Balance Charger
  <https://www.amazon.com/HOBBYMATE-Battery-Balance-Charger-Adapter/dp/B01NB9A36R/ref=sr_1_4?ie=UTF8&qid=1540908748&sr=8-4&keywords=lipo+charger>`_.
  NEVER CHARGE IT DIRECTLY CONNECTING IT TO A POWER SUPPLY (The charge makes
  chargin slopes that give the battery more capacity and life span).
  The battery is hanging in the front of the vehicle.
- Battery charger board: A battery charger board is included in the vehicle to be
  able to charge it from a laboratory power supply without the need of removing
  the battery from the vehicle.
- Fuel Gauge (optional): It measures the voltage and current of the battery.
  Using a propietary algorithm from Texas Instruments it measures the state
  of charge (percentage of remaining power) or the battery. It this board is
  not installed the main controller will not be able to read the state of
  battery of the device.
- WPT Secondary coil (optional): Coil to receive energy from the Wireless
  Power Transfer (WPT) system. Needed to charge the car wirelessly.
- WPT Secondary board (optional): Circuit that converts the signal from the
  WPT Secondary Coil and converts it into a DC voltage that can charge the
  battery (throuh the Battery charger board). Needed to charge the car wirelessly.
- Back panel: It is placed in the back of the vehicle. It contains the button
  to switch on the vehicle, a connector to select wired (using a laboratory
  power supply) or wireless (through the WPT system) charge, a fuse (that
  protects the system from overcurrent) and a LED that is red when the
  battery level is low.
- Arduino Romeo. This is an Arduino board with drivers for directly sourcing
  the motors. This program makes three different actions:

    - It reads the battery level (state of charge), battery voltage and battery
      current from the fuel gauge board (if installed).
    - It unplugs the battery in case the battery level goes beyond a safe levels.
      For this feature the fuel gauge needs to be installed.
    - It reads commands being sent by the main controller and executes them. The
      commands currently available are for changing the PWM of the left and right
      motors (and move the UGV) and reading the battery level (state of charge),
      battery voltage and battery current.

- In the top plate locate shape an identification. Currently red triangles are
  used to get the pose of the robot and a number is used for its identification.

The following image shows a UGV with some of the aforementioned elements:

..  image:: /_static/introduction-figs/pirate-4D-uvispace.png
    :width: 790px
    :align: center

The fuel gauge board and the WPT board go inside the chasis of the car (see
the UGV website for more details). The WPT coil goes under the car too.

Future work
-----------------

Some of the ongoing enhancements for UviSpace are:

- developing a graphical interface.
- building network controller using reinforcement learning. The current
  controller can pass through the set of points indicated through
  straight lines. Building a neural network controller will make the
  UGV be able to do any kind of trajectory.
- building a second UGV with Ackerman configuration. The controller for this
  vehicle can me the same as for the current differential vehicle. The reinforcement
  method will make the controllers to learn "alone" how to drive, regardless of the
  vehicle configuration.
- add support for automatic identification of the vehicles.
- add support for multivehicle, that is more than one vehicle being controlled at the
  same time. This brings the problem of designing a controller so the vehicles do not
  hit each other, or having a main trajectory planner that avoids the vehicles to
  pass the same point at the same time.
- to simulate different driving strategies for future real life environments
  involving self-driving cars like an automated parking lot or a factory with
  automated UGVs.

About us
--------

The project was developed by a team of researchers of the Microelectronics
division of the *Electronic Technology Department* of the `University of Vigo
<http://uvigo.gal/uvigo_en/index.html>`_. Contact information:

- Project directors: jjrdguez@uvigo.es and jfarina@uvigo.es
- Uvispace team e-mail: dte.uvispace@gmail.com

|

.. rubric:: **Bibliography**

.. [1] J.Rodriguez-Araujo, J.J.Rodriguez-Andina, J.Farina, and M.-Y.Chow, “Field-programmable system-on-chip for localization of UGVs in an indoor iSpace”, IEEE Trans. Ind. Informatics, vol. 10, no. 2, May 2014, pp. 1033-1043.

|
