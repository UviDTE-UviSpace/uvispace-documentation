Tutorials
=========

..  toctree::
    :maxdepth: 2

    Tutorials               <tutorials>


This page stores tutorials developed by the UviSpace collaborators.

PCB Manufacturing
-----------------

This is a tutorial of how to manufacture a PCB using a photosensitive board.
This process was used to build the prototypes of the UviSpace PCBs.

.. _photoengraving:

1 - Photoengraving
^^^^^^^^^^^^^^^^^^

This tecnique is used to transfer the pattern of the circuit to a board with
two cooper layers using UV light. The circuit for the two copper layers, front
and back, has to be printed in transparencies and atached in a way that you can
put inside the board. The light is going to react with the resin that covers
all the board except the zones just under the desired circuit. The
following picture shows the transparecies attached together and the UV machine
in the Department of Electronic Technology (DTE) in University of Vigo. Since
the black ink of our printer is not black enough for the UV we print two
transparecies by side (therefore we use 4 in total).

..  image:: /_static/tutorials-figs/pcb-manufacturing/photoengraving.jpg
    :width: 500px
    :align: center

.. note:: When printing the transparencies ensure that the drawing is printed
          in scale 1:1. Some programs (in particular PDF readers) tend to
          scale down the circuit a little bit. This scale-down is not
          appreciated by naked eye and you may realice after the board is
          manufactured when trying to solder the components.

The next step is to switch off the lights of the room and remove the protective
film (blue film  in the previous picture) of the board. The light can not be
switched on until the end of the process in order not to damage the
photosensitive resin. Put the board inside the transparencies. Handle it with
care in order not to touch, as much as possible, in order not to pollute the
copper.

After that, put the board in the insolator and program 150 seconds of light.

When the photoengraving has finished, the insolator can be openned and the board
taken outside the transparencies. Do not switch on the light yet.



.. _developing:

2 - Developing
^^^^^^^^^^^^^^^^^^^^^^^^

In order to eliminate all the resin that have reacted with the UV light, a
developing solution is used. There are many commercial developers. The most
common ara bags with small stones that are diluted in water.

Put the solution in a plastic tray. The plastic doesn´t react with the
quemical developer. The quantity has to be enough to cover all the board.
It is very important to keep the **lights switched OFF** during this step.

Once you have the tray full, put the board inside until you see the design of
the circuit. It is crucial do this step correctly but leaving the board in the
developing solution a time above the necessary has not bad consecuencies to the
process. In the UviSpace's PCB's manufacturing process a time of 5 minutes
was used. After that time the lights can be switched on. The board should
look something like this:

..  image:: /_static/tutorials-figs/pcb-manufacturing/developing.jpg
    :width: 500px
    :align: center

During the development process it is important not to leave the board on the
floor of the tray because the solution has to touch all the parts of the board.
It is also recommended to move the tray during the process and generate some
waves.

With the appropiate plastic gloves, the board can be rub few times to help the
developer remove the resin.

To finish with this step, the board can be softly cleaned with tap water.

After a visual checking, if a development error is detected, the board can be
introduced back again in the tray.

The development solution can be used again to process other boards or can be
stored back its bottle. Developing solutions can be reused several times.

.. _etching:

3 - Etching away the copper
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to etch away the copper of the PCB, a quemical solution has to be done
and put in a plastic tray. The plastic doesn´t react with this quemical
solution.

Classical recipe:

- 40 % of |H2O|
- 30 % of |H2O2|
- 30 % of HCl (concentration 20%)

In the DTE laboratory a more concentrated HCl concentration is used, therefore
the recipe is updated to:

- 55 % of |H2O|
- 30 % of |H2O2|
- 15 % of HCl (concentration 40%)

The order of the mixing process is very important and is the order that appears
in the reciepe. That is, firstly put the |H2O| in the plastic tray, after that
put the |H2O2| and finally the HCl.

.. |H2O2| replace:: H\ :sub:`2`\ O\ :sub:`2`\
.. |H2O| replace:: H\ :sub:`2`\ O

The following picture shows the |H2O2| used (left) and the process of mesauring
the volume needed using a graduated cylinder (right).

..  image:: /_static/tutorials-figs/pcb-manufacturing/h2o2.jpg
    :width: 550px
    :align: center

The following picture shows the measurment of the HCl in the graduated cylinder
(left) and the process of filling the tray (already with water and |H2O2|)
with HCl (right).

..  image:: /_static/tutorials-figs/pcb-manufacturing/hcl.jpg
    :width: 550px
    :align: center

Put the board in the tray and wait until the solution removes the cooper.
It is important for the PCB not to touch the bottom of the tray as it can
damage the resin on top of the cooper and the solution can remove cooper
from where it was not suposed to. You can use a piece of plastic for this
task like in the following picture:

..  image:: /_static/tutorials-figs/pcb-manufacturing/etching.jpg
    :width: 500px
    :align: center

Do this process under a extractor hood as the gas emanated by the process
is toxic. When all the traces seem to be clearly separated from each other
the board can be removed from the solution and cleaned with water.
At this moment the PCB can be visually checked. In case any of the
traces were wrong it could be re-introduced in the quemical solution to continue
the etching process.

When the board is ready it should be cleaned with water and some soap to stop
the chemical reaction. At this step the board should look something like this:

..  image:: /_static/tutorials-figs/pcb-manufacturing/etched-pcb.jpg
    :width: 500px
    :align: center

The quemical solution has to be deposited in the proper quemical container
for recycling. Do not spill the mixture through the sink.

The copper is still protected to the ambient conditions for a resin layer. It
has to be removed only when the components were ready to solder in order to not
allow copper to rust.

.. _drilling:

4 - Drilling component holes and vias
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In general terms, the appropiate moment to drill all the holes of the PCB is
before soldering the components. In the moment the PCB has no components,
nothing can be damaged.

.. _soldering:

5 - Soldering components
^^^^^^^^^^^^^^^^^^^^^^^^^

Now it's the turn of placing components but before doing that, the resin layer
that protects copper has to be removed. This is quite an easy process. Using
acetone and a piece of paper towel, remove all the resin from the PCB.
Be careful and not scratch the copper surface.

The board is now ready to recieve all the components and vias. First of all,
solder the smaller or more complicated component as small SMD
integrated circuits with many small pins. Secondly, solder the vias.
Introduce some wires and solder one side of the board. After that, solder the
other side, and finally cut the surplus wire. Finally, solder the rest of
the components both sides.

.. _varnishing:

6 - Varnishing
^^^^^^^^^^^^^^^

After soldering all the components and **test that the board correctly worksy**,
it is time to to protect the copper from the ambient. To do this,
in the DTE, specific barnish spray was used. Be aware the varinish is not
conductive so protect the parts of the design that you need a contact like
probe poits, etc. This can be made using paper tape.
