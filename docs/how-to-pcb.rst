Manufacture a PCB
=================

This is a tutorial of how to manufacture a PCB using a photosensitive board.

Table of contents:
------------------

| :ref:`1. Photoengraving <photoengraving>`
| :ref:`2. Developing <developing>`
| :ref:`3. Etching away the copper <etching>`
| :ref:`4. Drilling component holes and vias <drilling>`
| :ref:`5. Soldering components <soldering>`
| :ref:`6. Varnishing <varnishing>`

.. _photoengraving:

1 - Photoengraving
------------------

This tecnique is used to transfer the pattern of the circuit to the board using
UV light. The both copper layers, front and back, has to be printed in
transparencies and atached in a way that you can put inside the board. The light
is going to react with the resin that covers all the board except the zones just
under the desired circuit.

At that point, switch off the lights of the room and remove the protective film
of the board. The light can not be switched on until de development process. Put
the board inside the transparencies. Handle it with care in order not to touch,
as much as possible, the copper to not pollute it.

After that, put the board in the insolator and program 150 seconds of light.

When the photoengraving has finished, the insolator can be openned and the board
taken outside the transparencies.

.. _developing:

2 - Developing
--------------

In order to eliminate all the resin that have reacted with the UV light, a
developing solution is used.

Put the solution in a plastic tray. The plastic doesn´t react with this quemical developer. The quantity has to be enough to cover all the board. This step can be done at the beginning because now it is very
important **NOT** to switch on the lights.

Once you have the tray full, put the board inside until you see the design of
the circuit. It is crucial do this step correctly but leaving the board in the
developing solution a time above the necessary has not bad consecuencies to the
process.

In the UviSpace's PCB's manufacturing process a time of 5 minutes was used.
After 2 minutes it is completely safe switch on the lights again.

During the development process it is important not to leave the board on the
floor of the tray because the solution has to touch all the parts of the board.
It is also recommended to wave the tray during the process.

With the appropiate plastic gloves, the board can be rub few times to help the
developer remove the resin.

To finish with this step, the board can be softly cleaned with tap water.

After a visual checking, if a development error is detected, the board can be
introduced back again in the tray.

The development solution can be used again to other proces so that it can be
stored back in the same bottle it was.

.. _etching:

3 - Etching away the copper
---------------------------

In order to etch away the copper of the PCB, a quemical solution has to be done
and put in a plastic tray. The plastic doesn´t react with this quemical
solution.

**Recipe:**

40 % of *|H2O|*
30 % of *|H2O2|*
30 % of *HCl* (concentration 20%)

In the DTE laboratory other HCl concentration is used:

55 % of *|H2O|*
30 % of *|H2O2|*
15 % of *HCl* (concentration 40%)

The order of the mixing process is very important and is the order that appears
in the reciepe. That is, firstly put the |H2O| in the plastic tray, after that
put the |H2O2| and finally the HCl.

.. |H2O2| replace:: H\ :sub:`2`\ O\ :sub:`2`\
.. |H2O| replace:: H\ :sub:`2`\ O

The etching process time depends on the quemical reaction. When all the traces
seemed to clearly separated from each other the process can be stopped using tap water. At this moment the PCB can be visually checked. In case any of the
traces were wrong it could be re-introduced in the quemical solution to continue
the etching process.

When the board is ready, the rest of the solution have to be cleaned with tap
water and some soap. The quemical solution has to be deposited in the proper
quemical container.

The copper is still protected to the ambient conditions for a resin layer. It
has to be removed only when the components were ready to solder in order to not
allow copper to rust.

.. _drilling:

4 - Drilling component holes and vias
-------------------------------------

In general terms, the appropiate moment to drill all the holes of the PCB is
before solder the components. In the moment the PCB has no components, nothing
can be damaged.

.. _soldering:

5 - Soldering components
------------------------

Now it's the turn of placing components but before doing that, the resin layer
that protects copper has to be removed.

This is a quite easy process. Using acetone and a piece of paper towel, remove
all the resin from the PCB. Take care of not scratching the copper surface.

The board is now ready to recieve all the components and vias.

**First of all**, solder the smaller or more complicated component as small SMD
integrated circuits with many small pins. **Secondly**, solder the vias.
Introduce some wires and solder one side of the board. After that, solder the
other side, and finally cut the surplus wire. **Finally**, solder the rest of
the components both sides.

.. _varnishing:

6 - Varnishing
--------------

After solder all the components and **test if the board works correctly**, is
the turn of protecting again the copper from the ambient conditions. To do this,
in the DTE, specific barnish spray was used.
