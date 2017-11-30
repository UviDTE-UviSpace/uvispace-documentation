Tutorials and manuals
=====================

..  toctree::
    :maxdepth: 2

    Tutorials               <tutorials>

Altera Quartus and SOPC-Builder
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The FPGA project was developed using *Quartus II* and *SOPC-Builder v12.1*.
*Quartus II* is a design software for the hardware for Altera FPGAs.
*SOPC-Builder* is a tool inside *Quartus II* for easily developing systems on
chip (SoCs) i.e. digital systems with all or almost all elements (processor,
memory and peripherals) in a single chip. Newer versions  of *Quartus II*
(after v13) use an evolved variant of this tool, called *Qsys*, and both
*SOPC-Builder* and *Qsys* are tipically used to develop systems based on the
soft-processor *Nios II*. They use **Avalon** standard buses to ease the
development of SoCs:

- **Avalon Memory-Mapped (MM):** It contains the signals needed to develop a
  master-slave bus e.g. Adress Bus, Data Bus, READ, WRITE, etc. There are two
  tipes of *Avalon MM* interfaces, namely *Avalon MM slave* and *Avalon MM
  Master*. The communication is always initiated by the masters ordering/writing
  to or asking/reading from the slave.
- **Avalon Streaming (ST):** It is a point to point connection for fast data
  transmission. There are two types, depending if they are the source or the
  sink of data.
- **Avalon Interrupt:** It is a point to point connection between two systems,
  one generating an interrupt and another attending it.
- **Clock:** It is a point to point connection between a clock signal source
  and a clock signal sink.
- **Reset:** It is a point to point connection between a reset source and its
  sink.
- **Avalon Tri-state Conduit (TC):** For tri-state controllers. It allows the
  connection of different components with third state features, using small
  number of signals, saving pins and space in the board.
- **Conduit:** It is a point to point connection between two conduit signals.
  It is used when none of the previous interfaces can be used. It is typically
  used to export signals outside of *Qsys* and represent the particular
  interface of some chip. It is also used to connect two components with custom
  user-defined interface between them.

Using the standard buses makes the connection between components fast because
*Qsys* "knows" their signals and its meaning. In a single click a memory mapped
slave can be connected to a processor, saving time and avoiding errors.
*SOPC-Builder* and *Qsys* create a lot of logic automatically. For example,
*Avalon MM* buses create the logic circuits needed to implement the desired
address map. *SOPC-Builder* and *Qsys* also have a big library of IP
(Intellectual Property) cores that are tested and documented. This permits the
developer to focus his efforts on the components that are specific to the
application.

FPGA Hardware compilation  and file organization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following steps show how to set up the FPGA project:

- **Edit the Nios II System using the SOPC-Builder.**

- **Save and Generate the SOPC-Builder project**. After that, several files
  will be created in the project folder:

    - *DE2_115_SOPC.sopcinfo:* XML file containing the structure and settings
      for the components in the *Nios II* System. This file is used by *Nios II
      EDS* to know which hardware is inn the system and ease *Nios II*
      programming.
    - *DE2_115_SOPC.v:* Main HDL file for the *Nios II* system, that
      instantiates all the IP cores that were previously added in the
      *SOPC-Builder*.
    - The rest of HDL files for Nios II system are *.v* files copied into the
      project folder. They are the source files of all of the components added
      in *SOPC-Builder*. Altera library IP cores (like Flash Controller, SRAM
      Controller, LCD Controller, Timer, Sys_ID) are copied from the Altera
      installation folder. The rest of IP cores (Avalon Camera, APIO Sensor,
      Trackers and Ethernet Controller) are copied from  folders inside the
      project folder (*ip/Avalon_camera, ip/apio_sensor, ip/Avalon_locator* and
      *ip/eth_ocm*).

- **Instantiate the Nios II System and other components in Quartus II**. The
  *Nios II* system, represented now by *DE2_115_SOPC.v* is a component and can be
  used in *Quartus II* designs using *DE2_115_SOPC* as component name. To use the
  *Nios II* system and connect it to other components, the file
  *DE2_115_WEB_SERVER.v* is created. It defines the main entity for the FPGA
  hardware project. The name of this entity should be the same name as the
  project name because Quartus II starts the compilation looking for the entity
  with the same name as the project. Inside the script, the *Nios II* System
  (DE2_115_SOPC) is instantiated along with the other components in the *Quartus
  II* part of the system (orange part of the diagram), i.e. *Camera controller,
  Sensor controller,  SDRAM Controller, VGA Controller* and *Display Controller*.
  The source files for these components are in subfolders *camera, display* and
  *sensor* inside the project folder. In *DE2_115_WEB_SERVER.v* the components
  are connected to the FPGA pins using names (LEDR, CLOCK_50, etc.). The relation
  between these names and the actual pins on the chip is in the
  *DE2_115_WEB_SERVER.qsf*. This file can be generated for any DE2-115 board
  using the System Builder program in the DE2-115 CD-ROM documentation.

- **Add compilation files to Quartus II**. All HDL files used in the project
  should be added to the files tab in Quartus II (left upper part of the program),
  so the compiler can find them. The following files should be added:

    - *DE2_115_WEB_SERVER.v:* It is the main file containing the highest level
      entity.
    - *DE2_115_SOPC.v:* This file contains the definition of the *Nios II*
      system.
    - *DE2_115_SOPC.quip:* This file imports the rest of the files which
      describe the components used in *SOPC-Builder* that form the *Nios II*
      System. *.quip* files are used for importing a group of HDL files
      together.
    - The rest of *.quip* files for the *Quartus II* part of the system (orange
      part in the diagram), namely *display/vga.quip, display/display.quip,
      camera/camera.quip, etc.*

- **Add libraries**. the source files of *eth_ocm* component are added as a
  library. Hence, add it in *Quartus II->Assignments->Settings->Libraries*.

- **Compile the project**. Select *compile design* or *Processing-> Start
  Compilation*. Firstly, *Analysis&Synthesis* converts HDL design to the FPGA
  hardware. Then, the *Fitter* does the *Place & Route* of the circuit (locates
  the circuit in the specific device assigned to the project and interconnects
  all the elements). Finally, the *Assembler* generates the *.sof* file used
  to program the FPGA. After that, the program should look similar to the
  figure below.

..  image:: /_static/quartus-compilation.png
    :width: 750px
    :align: center


Importing the software project
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Usually, when one starts a new project, both subprojects are generated at the
same time using *File->New Nios II Application and BSP From Template*. However,
in this case the subprojects are already created so they have to be imported
into the workspace. To do this go to *File->Import->General->Existing Projects*
then go to *workspace->Select root directory [project folder]/software* and
click *copy projects into workspace*. Another recommended way is to choose
*[project folder]/software* as the project workspace, being not necessary to
copy the subprojects into the workspace if doing so.

Changing sopc_info file location in the BSP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The BSP uses global location to find the *.sopc_info* file. When changing the
location of the folder, the path of the *.sopc_info* file has to be changed
before generating the BSP again. It can be done in the *settings.bsp* file
inside the BSP project under the *<SopcDesignFile>* entry.

Software project compilation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The steps required to compile the system are the following:

- **Generate BSP:** Right click at *Socket_Server_bsp->Nios II->Genrate BSP*.
- **Compile BSP:** Right click at  *Socket_Server_bsp->Build*.
- **Compile application:** Right click at *SocketServer-> Build*. The makefile
  combines the BSP and application compilations to generate a single output
  *.elf* file located inside the application folder. In this case it is called
  *SocketServer.elf*. This file is the program for the *Nios II* processor.

In the figure below, it can be observed how will look the EDS after opening the
application and BSP projects.

..  image:: /_static/fpga-software-eds.png
    :width: 750px
    :align: center

How to load hardware and software in the FPGA board
---------------------------------------------------

There are 2 different options:

- **While debugging:** In this case the board is connected to the PC for
  debugging. The console in the *Nios II EDS* can be used. In this situation both
  the program and data of the *Nios II* processor are stored in the SRAM memory.
  The DE2_115_sopcbuilder12 design accomplish this task:

    - In the *Nios II* processor component in *SOPC-Builder* reset and exception
      vectors have to point to the *SRAM Controller* component (so the SRAM
      becomes its program memory).
    - Compile the SOPC Builder project and compile using Quartus II.
    - Set the RUN-PROG switch in RUN position and switch on the board.
    - Load the DE2_115_WEB_SERVER.sof file into the FPGA using the Quartus
      Programmer in JTAG mode.
    - With the BSP pointing to the last version of the DE2_115_SOPC.sopcinfo
      Generate the BSP and recompile the BSP and software projects.
    - Load the SocketServer.elf into the SRAM memory doing: Nios II EDS->Right
      click SocketServer->Run as->Nios II Hardware. The first time you load a
      .elf you have to configure the loading processes using : Right click
      SocketServer->Run configurations. There the board can be detected.
      Uncheck Ignore mismatched System ID or ignore mismatched system timestamp
      if you are sure that .elf is targeting the board connected (sometimes it
      fails even the software project is generated from the .sopc_file of the
      system in the board).
    - The program is loaded and it starts. You can use the console as std input
      and output.

- **Stand-alone board:** (THIS STEPS DON'T WORK) In this case the program of the
  Nios II is loaded into the non-volatile flash memory and the configuration of
  the FPGA is loaded into the non-volatile EPCS memory available in the board.
  When the board is configured using this option, during the start-up the FPGA
  is loaded from the EPCS and the Nios II starts reading its program from the
  flash memory, that is now its program memory. The SRAM memory is one more
  time the data memory of the processor. This is the mode used to put the
  cameras in the ceiling of the lab. We provide this version in the
  DE2_115_sopcbuilder12_flash folder. The steps to do that:

    - Delete old version of DE2_115_sopcbuilder12_flash.
    - Take the DE2_115_sopcbuilder12 project and copy it, change the name to
      the copy by DE2_115_sopcbuilder12_flash.
    - In the Nios II processor component in SOPC Builder set the reset and
      exception vectors pointing to the Flash controller component (now the
      Flash component is the program memory of the system).
    - Compile the SOPC Builder project and compile using Quartus II.
    - Convert the DE2_115_WEB_SERVER.sof file into DE2_115_WEB_SERVER.pof file
      using Quartus II->File->Convert Programming file. It is generated in the
      project folder. Choose EPCS64 as device and Active programming as
      settings, and DE2_11_WEB_SERVER.sof as input file.
    - Set the RUN-PROG switch in PROG position and switch on the board.
    - Load the DE2_115_WEB_SERVER.pof file into the EPCS memory using Quartus
      II programmer in Active-Serial mode.
    - Switch off the board, switch the RUN-PROG switch to RUN position and
      switch on the board again. Now the FPGA is configured from the EPCS.
    - The previous .elf used in debug mode can be used. However, if you need
      to recompile it generate the BSP and recompile the BSP and software
      projects again.
    - In the Nios II EDS right click in SocketServer project->Nios II->Flash
      programmer. Click File->New->Get Flash programmer system from SOPC
      Information File->Select the .sopc_info file generated in the last
      compilation (the one where Nios II reset and exception vectors are
      pointing to the flash memory) ->click OK, add the SocketServer .elf file
      and press start. The .elf file should be downloaded into the flash.


Migrate the hardware project to Qsys
------------------------------------

An attempt to migrate this code to Qsys was done. Using Qsys instead of SOPC
Builder permits to use the newer versions of Quartus like Quartus II. Otherwise
old Quartus versions have to be used. The last version with SOPC Builder is the
Quartus II 12.1. This version contains both Qsys and SOPC Builder and permits
the migration from SOPC Builder to Qsys.

Several attempts were done to migrate the design to Qsys without success.

- First the less number of modifications to the design were done. The design was
  open with Qsys and migrated to Qsys. The SOPC Builder files are automatically
  sent to a Back Up. In this process Qsys erases all .quip files in Files Tab of
  Quartus II. Qsys saves all data in DE115_SOPC/systhesis folder. The
  DE115_SOPC/systhesis folder DE115_SOPC.qip is added to Quartus. The rest of
  .qip files for the components added in Quartus II (like sensor or camera) are
  added too. The design compiles. The processor works but Ethernet do not. Some
  file is probably missing in the compilation. The compilation process does not
  break but the design is not fully completed.

- Starting from zero. The design was  open with Qsys, regenerated. All files in
  Files tab in Quartus II are removed. In this scenario when the compilation
  crashes because files are not found, they are supplied one by one. The project
  compiles but the end result seems worse than before because the program cannot
  be loaded into the Nios II processor.

In this scenario few more attempts to directly from SOPC Builder to Qsys can be
done. If there is no success the best option is to start from bottom a new
project and go adding functionality to it. First the Nios II and its memories.
If it works, Ethernet capabilities can be added, and so on. The error probably
is in some files missing for the Ethernet controller or some files missing in
one of the multiple IP cores added with Quartus and the IP Wizard. This Wizard
IPs usually come with some files to configure some tables of data needed to do
the functionality of the IP. If this is the case the compiler can compile
leaving that memories empty and later the IP core functionality is wrong.




Manufacture a PCB
^^^^^^^^^^^^^^^^^

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



Create documentation
^^^^^^^^^^^^^^^^^^^^

| :ref:`1. Installing Python in Windows <python-windows>`
| :ref:`2. Installing the Virtual Environment <virtual-env>`
| :ref:`3. Creating a new virtual environment <new-virtual-env:>`
| :ref:`4. Installing Sphinx <sphinx>`

.. _python-windows:

1 - Installing Python in Windows
--------------------------------

Python can be downloaded here_.

.. _here: https://www.python.org/downloads/

* For Windows 32-bit just click on the Download Python button (version 2).
* For `Windows 64-bit`__. Download the *x86 executable installer* under the
  **Files** heading.

__ 64-bit_

.. _64-bit: https://www.python.org/downloads/release/python-363/

During the installation process take care that the option *installing pip* is
checked.

.. _virtual-env:

2 - Installing the Virtual Environment
--------------------------------------

In order to ease the management of the project dependencies, it is recommended
to make use of Python virtual environments. Virtual environments are used to
provide isolation between projects, keeping all the required libraries in a
directory available only to the virtual environment, and thus avoid polluting
the global sites-packages directory.

There is a utility called virtualenvwrapper that simplifies the management of
virtual environments, and we will use this utility to obtain a clean virtual
environment for our project.

To install this utility, run the following command (root permissions may be
necessary):

.. code-block:: bash

   $ pip install virtualenvwrapper - win


.. _new-virtual-env:

3 - Creating a new virtual environment
--------------------------------------

To create a new virtual environment, run the following command:

.. code-block:: bash

   $ mkvirtualenv name

Note that "name" has to be sustituted by the virtual environment name.

.. _sphinx:

4 - Installing Sphinx
---------------------

Sphinx is a tool to create documentation.
To install it, run the following command:

.. code-block:: bash

   $ pip install sphinx
