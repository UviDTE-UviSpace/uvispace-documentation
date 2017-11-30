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
|   :ref:`4.2 Software <fuel-gauge-software>`
| :ref:`5. Charger <charger>`
| :ref:`6. Wireless power transfer <wpt>`
|   :ref:`6.1 Primary Hardware <wpt-primary-hardware>`
|   :ref:`6.2 Secondary Hardware <wpt-secondary-hardware>`
|   :ref:`6.3 Primary Software <wpt-primary-software>`

.. _vehicle:

1 - Vehicle
-----------

.. _pirate-4dw:

Pirate-4WD Mobile Platform
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. This section will include the DFRobot specifications as well as 2 photos of
   the UGV without top neither bottom pannels and the names of all the boards.
   Also an screenshot of the schematic that will be included in the pcb-designs
   repository to specify the wirings and the set up of the robot.

`Pirate-4WD Mobile Platform`__ is a robotics platform of the DFRobot_ brand. It is the first robot used in the UviSpace.

__ pirate_

.. _pirate: https://www.dfrobot.com/product-97.html
.. _dfrobot: https://www.dfrobot.com/

The power supply of the robot is a Lithium Polymer battery. This battery has a voltage of 7,4 V (2 cells of 3,7 V each) and 2200 mAh of capacity that supplies power to the 4 DC motors (one per wheel) and to the `UGV controller`__.

__ arduino_

It also has some PCB's. Fuel gauge PCB monitors few battery parameters as the **State Of Charge**, the *voltage* or the *temperature*. Theese values are transmitted via I2C protocol to the UGV controller. The battery charger PCB allow to charge the battery either using an external power supply or the Wireless Power Transfer System.
The controller is an Arduino board that receive commands via wireless using a Zigbee module from the main controller. It has two main functions. One, transmit the battery parameters to the main controller so that it can know wheter the battery is in good conditions to continue working, and the other is deal with the navigation of the UGV.

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

.. _fuel-gauge-software:

Software
^^^^^^^^

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
