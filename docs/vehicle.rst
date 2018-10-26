UGV
===

Table of contents:
------------------

| :ref:`1. Vehicle <vehicle>`
|   :ref:`1.1 Pirate-4WD Mobile Platform <pirate-4wd>`
| :ref:`2. Arduino Romeo v1.1 <arduino>`
|   :ref:`2.1 Hardware <arduino-hardware>`
|   :ref:`2.2 Software <arduino-software>`
| :ref:`3. Communications Board <communications>`
|   :ref:`3.1 Communications Shield <communications-shield>`
|   :ref:`3.2 ZigBee Module <zigbee>`
|   :ref:`3.3 WiFi Module <wifi>`
| :ref:`4. Fuel gauge <fuel-gauge>`
|   :ref:`4.1 Hardware <fuel-gauge-hardware>`
|   :ref:`4.2 Firmware configuration <fuel-gauge-fw-config>`
| :ref:`5. Charger <charger>`
| :ref:`6. Wireless power transfer <wpt>`
|   :ref:`6.1 Primary Hardware <wpt-primary-hardware>`
|   :ref:`6.2 Secondary Hardware <wpt-secondary-hardware>`
|   :ref:`6.3 Primary Software <wpt-primary-software>`

.. _vehicle:

1 - Vehicle
-----------

.. _pirate-4wd:

Pirate-4WD Mobile Platform
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. This section will include the DFRobot specifications as well as 2 photos of
   the UGV without top neither bottom pannels and the names of all the boards.
   Also an screenshot of the schematic that will be included in the pcb-designs
   repository to specify the wirings and the set up of the robot.

`Pirate-4WD Mobile Platform`__ is a robotics platform of the DFRobot_ brand. It
is the first robot used in the UviSpace.

__ pirate_

.. _pirate: https://www.dfrobot.com/product-97.html
.. _dfrobot: https://www.dfrobot.com/

The power supply of the robot is a Lithium Polymer battery. This battery has a
voltage of 7,4 V (2 cells of 3,7 V each) and 2200 mAh of capacity that supplies
power to the 4 DC motors (one per wheel) and to the `UGV controller`__.

__ arduino_

It also has some PCB's:

- *Fuel gauge PCB* monitors few battery parameters as the
**State Of Charge**, the *voltage* or the *temperature*. Theese values are
transmitted via I2C protocol to the UGV controller.

- *Battery charger PCB* allow to charge the battery either using an external
power supply or the Wireless Power Transfer System.

- The *controller* PCB has an `Arduino all-in-one controller`__. This board
communicates with the *main controller* in order to deal with two main tasks:

One, transmit the battery parameters to the main controller using a
*Zigbee module* so that it can now if the battery is in good conditions to
continue working, and the other is to deal with the navigation of the UGV.

__ arduino_

- *Zigbee module*


.. _arduino:

2 - Arduino Romeo v1.1
----------------------

This is an all-in-one controller board. In this section it will be explained all
the important features of the board as well as the programs that implements in
the UviSpace.

Link to external web where it can be found all the documentation.

.. _arduino-hardware:

Hardware
^^^^^^^^

.. _arduino-software:

Software
^^^^^^^^

In the arduino-UGV-controller repository complete information can be found.

.. _communications:

3 - Communications Board
------------------------

.. _communications-shield:

Communications Shield
^^^^^^^^^^^^^^^^^^^^^

.. _zigbee:

ZigBee Module
^^^^^^^^^^^^^

Hardware
""""""""

Software
""""""""

.. _wifi:

WiFi Module
^^^^^^^^^^^

Hardware
""""""""

Software
""""""""

.. _fuel-gauge:

4 - Fuel gauge
--------------

Some introduction to the functionalities that board implements and explain the
reasons that make this board necessary.

.. _fuel-gauge-hardware:

Hardware
^^^^^^^^

This section includes all the versions of the fuel gauge PCB that pretends to
show the complete history of this board from the very beginning, explaining
the first design with all the details and design considerations, to the latest
design, including all the new features implemented and tested.

1 - Version 1.0
"""""""""""""""

Here explain the first version that is the one on the Degree thesis included in
the pcb-designs repository, inside fuel gauge folder.

2 - Version 1.1
"""""""""""""""

In this PCB version the v1.0 was modified including:

* The I2C jumpers in order to connect either the PC or the ZigBee to the IC.
* Rsense traces improved: width of traces modified with tin in order to minimize
the voltage drop in this critical part of the circuit.

3 - Version 2.0
"""""""""""""""

In this PCB version the v1.1 was modified including:

* Safety shutdown ciruit
* I2C replacement by a SPDT switch
* Number of PCB vias reduced dramatically

.. _fuel-gauge-fw-config:

Firmware configuration
^^^^^^^^^^^^^^^^^^^^^^

In this section has to be specified how to program all the registers of the IC
and how to do the calibration. Also explained the issues that we have found
when doing that process the first times. In the past we used the bq34z100. That
chip had an error with the alert and it didn't work. We purchase a new design:
the bq34z100-G1. In the voltage calibration process we had allways the same
error. Later on, we've discoverde the problem was the software used was
corrupted. Once we update the software to the latest build, the calibration was
perfect.

Battery Management Studio (bqStudio) Software v1.3.80 Build 1 	28-SEP-2017
http://www.ti.com/tool/bqstudio?keyMatch=bqstudio&tisearch=Search-EN-Everything

bq34z100EVM Wide Range Impedance Track™ Enabled Battery Fuel Gauge Solution
http://www.ti.com/lit/ug/sluu904a/sluu904a.pdf
A guide of how to calibrate the fuel Gauge

Drivers for EV2300 can be foud in the repository

.. _charger:

5 - Charger
-----------

.. _wpt:

6 - Wireless power transfer
---------------------------

.. _wpt-primary-hardware:

Primary Hardware
^^^^^^^^^^^^^^^^

.. _wpt-secondary-hardware:

Secondary Hardware
^^^^^^^^^^^^^^^^^^

.. _wpt-primary-software:

Primary Software
^^^^^^^^^^^^^^^^







-------------------------UGV explanation Javi------------------------------
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
-------------------------UGV explanation Javi------------------------------
