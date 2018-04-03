Wireless Power Transfer
======================

..  toctree::
    :maxdepth: 3

    Wireless Power Transfer        <wpt>

Introduction
------------
This documentation page explains a Wireless Power Transfer system that can be used to charge the UGVs in Uvispace. When UGV battery is getting low the Main Controller may decide to take the car to a Wireless Charging Station, therefore providing a way to continuously work without human intervention. This could be useful to simulate interesting scenarios in real life environments like a factory that uses UGVs for material transportation or a parking that charges the parked vehicles so the battery is full when the owner comes back.




WPT Hardware
------------

Overview
^^^^^^^^^
The Wireless Power Transfer (WPT) System is composed by two or more devices. There is at least one emitter and one receiver (as a primary and a secondary in a conventional transformer), though the number of these can be increased depending on the topology. In this project the SISO (Single Input Single Output) topology will be used.

The function of the primary is to transform the input signal in an AC with high frequency (from kHz to MHz), in the following section the blocks and topologies used will be explained. The secondary's function is to receive that high-frequency signal and to transform it into a signal capable of charging a battery efficiently.

(Explain that there is a primary and a secondary, explain a little bit its working, put a general picture or diagram of the system. Point out to the repositories where the hardware can be found. Explain that a in the TFG a detailed description is given)

Primary
^^^^^^^
Small explanation of the different parts of the primary. Send the user to the TFG for more details.
Important to write here the differences if any with the TFG.
Write here too the functionality that is available.

Secondary
^^^^^^^^^
Small explanation of the different parts of the primary.

Primary coil
^^^^^^^^^^^
How to design a new coil.

Secondary coil
^^^^^^^^^^^^^
How to design a new coil.



WPT Software
------------
Explain were to find the software.
Explain what it does in few lines. Point out what features are included and which ones do not.


WPT Charger Installation
------------------------
Explain in few steps how to build it:

* Manufacture the boards: Send the user to the PCB tutorial.

* Solder components. Sope clarification on how to adjust the capacitors to match resonance and things like that.

* Manufacture secondary.

* Solder secondary.

* Mount the secondary in the car. Send to UGV wiring repo that explains the connection. Maybe add a pic here.

* Installa a platform to make the car go over the primary coil. Add a picture of it when it is finished. (Roberto will do it when the wood box is finished.)
