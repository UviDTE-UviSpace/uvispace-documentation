UGV
===

Table of contents of this file:

| :ref:`1. Vehicle <vehicle>`
|   :ref:`1.1 DFRobot <dfrobot>`
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

.. _dfrobot:

DFRobot
^^^^^^^

This section will include the DFRobot specifications as well as some designs to
specify the wirings and the set up of the robot.It also includes all the parts
of the robot well referenced and may have some photos.

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
