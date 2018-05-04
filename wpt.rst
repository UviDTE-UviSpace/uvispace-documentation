Wireless Power Transfer
======================

..  toctree::
    :maxdepth: 3

    Wireless Power Transfer        <wpt>

Introduction
------------
This documentation page explains a Wireless Power Transfer system that can be used
to charge the UGVs in Uvispace. When UGV battery is getting low the Main Controller
may decide to take the car to a Wireless Charging Station, therefore providing a
way to continuously work without human intervention. This could be useful to simulate
interesting scenarios in real life environments like a factory that uses UGVs for
material transportation or a parking that charges the parked vehicles so the battery
is full when the owner comes back.

WPT Hardware
------------

Overview
^^^^^^^^^
The Wireless Power Transfer (WPT) System is composed by two or more devices. There
is at least one emitter and one receiver (as a primary and a secondary in a
conventional transformer), though the number of these can be increased depending
on the topology. In this project the SISO (Single Input Single Output) topology
will be used.

The function of the primary is to transform the input signal in an AC with high
frequency (from kHz to MHz), in the following section the blocks and topologies
used will be explained. The secondary's function is to receive that high-frequency
signal and to transform it into a signal capable of charging a battery efficiently,
i.e. a CC signal with a specific voltage level.

In the Figure an overview of the blocks is available.

..  image:: /_static/WPT/system_block_diagram.png
    :align: center

.. (Explain that there is a primary and a secondary, explain a little bit its working,
  put a general picture or diagram of the system. Point out to the repositories
  where the hardware can be found. Explain that a in the TFG a detailed description
  is given)

Primary
^^^^^^^
To transfer efficiently energy over an air gap in a WPT system, the frequency of
the emitter's signal has to be significantly high. With higher frequency, higher
coupling coefficient and hence, higher efficiency. The primary is the responsible
for rising the frequency of 50 Hz (EU) to tens or hundreds of kHz.

..  image:: /_static/WPT/primary_block_diagram.png
    :align: center

The first block is a rectifier followed by a CC/CC regulator, to convert the
AC input signal into a controlled CC with an appropiate voltage level. In this
project, this block is implemented with a commercial computer power adapter, assuring
a 24 V signal at the input of the next block.

After the regulator is places the inverter. This blocks transforms the CC signal
into a high frequency AC signal. In this project a full bridge (H-bridge) inverter
is used. Unfortunately, the output of this device is a square signal, i.e. a fundamental
frequency with a lot of higher frequency harmonics. The harmonic analysis can be
found on the repository (TFG-wpt-system). This harmonics would mean a substancial
quantity of noise in the air-core transformer, so this signal has to be filtered.

The filters used for the purpose are LC resonant tanks. The explanation of the
behavior of resonant tanks can be found on the TFG, but as a brief summary, a
series LC tank behaves as a band-pass filter and a parallel LC tank
behaves as a band-stop filter at resonance. The filter used is a composing of both
topologies, a series LC tank tuned at the input signal's frequency, blocking the
high-frequency harmonics, followed by a parallel LC tank that stores the energy
of the fundamental frequency, having a small current outside the tank, and a high
current inside. The inductor of the parallel LC tank is the emitter coil.

..  image:: /_static/WPT/resonant_tanks.png
    :align: center

The last part of the primary is the digital circuit capable of generating the signals
needed by the inverter's MOSFET transistors. The following block diagram shows the
composition of this circuit.

..  image:: /_static/WPT/digital_system_block_diagram.png
    :align: center

The main block in this system is the microcontroller, a PIC18F45K20 is used in this
project. His work is to generate the two square signals for the inverter, and to
assure that no overlap exists. This signals are connected to a pair of drivers (IR2110)
in order to adapt the voltage of the signals and to assure an isolation between the
upper part of the bridge and the lower part.

The microcontroller has more tasks to be implemented. Among them, a XBee module
to activate the charger remotely, two LEDs to show if the charger is powered and
if it's activated manually or remotely and two switches to select which mode is used.

The last block is the PICkit 3 interface, making possible to program the microcontroller
when it's in the PCB using the PICkit 3 tool.

More information about each block can be found on the TFG available at the
repository.

.. Small explanation of the different parts of the primary. Send the user to the TFG for more details.
  Important to write here the differences if any with the TFG.
  Write here too the functionality that is available.

Secondary
^^^^^^^^^
The first part of the secondary circuit is the receiver coil, which takes energy
from the primary coil. In this topology, the two coils has to be as close as possible
and vertically aligned. As in the primary, the secondary receiver is part of a
series resonant tank tuned at the system's frequency.

After the resonant tank, there is a rectifier and a regulator that transform the
AC signal into a CC signal capable of charging the battery. The voltage level of
this signal is 12 V.

..  image:: /_static/WPT/secondary_block_diagram.png
    :align: center

Primary coil
^^^^^^^^^^^^^
The design process  of a WPT system has many degrees of freedom. The decision made
in this project is that the first parameter to be fixed is the primary coil. The
first step in this design is the cable selection.

The selection of the cable diameter is based on the current through the coil.
There are many tables that allows to select a cable with this parameter, but as
this circuit works with high frequencies, the skin effect has to be considered.
This principle stablishes that the current only flows in the exterior layers of
a wire when working above a determined frequency, and the depth of this layer is
decreases with the frequency's value. For more information about this, refer to
the TFG on the repository.

The cable used has to be of multiple wires in most cases. The diameter of these
wires cannot exceed twice the skin depth value, and must endure the current flowing
through the primary in all cases. The skin depth value can be known using a calculator
available at http://chemandy.com/calculators/skin-effect-calculator.htm.

Once the cable has been selected, it's possible to begin with the coil construction.
First of all, the space that the coil will occupy has to be delimited. This has
importance when choosing the topology. In this project, due to the limitations
of vertical space, a spiral coil has been designed.

The formulas regarding the primary coil inductance are available in the TFG at
the repository, the rule followed in this project is to have as many turns as
possible to increase the inductance, occupying the maximum space available. Once
the coil is constructed, its inductance can be obatained using an inductance
calculator or formula depending on the coil shape. There are several books on
this subject and many online calculators as well.

In summary, the steps for building a new coil are the following:

* **Selecting a cable**: The cable has to be of multiple wires to deal with the skin effect.

* **Delimiting the coil space**: Necessary to reduce the degrees of freedom.

* **Selecting a coil shape**: Depending on the space available and other limitations.

* **Building the coil**: More inductance with more turns.

* **Obtaining the coil inductance**: Using formulas or online calculators.

Secondary coil
^^^^^^^^^^^^^^^
The secondary coil has to be designed after the primary coil. It's shape has to
be the same, or similar, as the primary. Although It does not have to be the same
topology. In this project the primary is a square-shaped spiral and the secondary
is a square-shaped coil. The size of the secondary has to be a bit smaller than
the primary, this way a good coupling coefficient can be achieved.

The construction of the secondary is based on trial and error. The turns of the
secondary coil have to be changed until a suitable range of voltage is obtained.
This range of voltage is based on the range of operation of the regulator.

WPT Software
------------
The software used in this design is divided in two parts, the microcontroller's
program and the PCB design.

The microcontroller's program is available on the TFG at the repository, and the
only function it covers by the time is the signal generation for the inverter.

The PCB design covers the schematics and PCB layout designed in KiCAD, it's
available at the repository as well.

.. Explain were to find the software.
  Explain what it does in few lines. Point out what features are included and which ones do not.


WPT Charger Installation
------------------------
The steps to be followed when installing the charger are these:

* **Manufacture the boards**: Send the user to the PCB tutorial.

* **Design the coils**: As said above, the inductance of the coils are needed for the capacitor selection in the primary and the secondary.

* **Solder components**: Once the capacitors are selected, matching the resonance with the inductance of the coils at the system's ferequency, the component soldering can begin.

* **Mount the secondary in the car**: Send to UGV wiring repo that explains the connection. Maybe add a pic here.

* **Install a a platform to make the car go over the primary coil**: Add a picture of it when it is finished. (Roberto will do it when the wood box is finished.)
