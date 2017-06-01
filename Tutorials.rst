Tutorials and manuals
=====================

..  toctree::
    :maxdepth: 2

    Tutorials


Python virtual environments
^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to ease the management of the project dependencies, it is recommended
to make use of Python virtual environments. Virtual environments are used to
provide isolation between projects, keeping all the required libraries in a
directory available only to the virtual environment, and thus avoid polluting
the global sites-packages directory.

There is a utility called **virtualenvwrapper** that simplifies the management
of virtual environments, and we will use this utility to obtain a clean virtual
environment for our project.

To install this utility, run the following command (root permissions may be
necessary):

.. code-block:: bash

    $ pip install virtualenvwrapper

Once the installation has finished, open up the *.bashrc* file and add the
following lines:

.. code-block:: bash

    export WORKON_HOME=$HOME/.virtualenvs      # Virtual environments folder
    source /usr/local/bin/virtualenvwrapper.sh # Enable virtualenvwrapper commands

To create a new virtual environment, run the following command:

.. code-block:: bash

    $ mkvirtualenv uvispace # Syntax: mkvirtualenv (name for the virtual environment)
    (uvispace) $            # Prompt gets updated to reflect the virtual environment is active

In case the virtual environment is already created, it is only necessary to
activate it with the following command:

.. code-block:: bash

    $ workon uvispace # Syntax: workon (name for the virtual environment)
    (uvispace) $      # Prompt gets updated to reflect the virtual environment is active

If we want to stop using a virtual environment, run the following command:

.. code-block:: bash

    (uvispace) $ deactivate
    $                        # Prompt gets updated to reflect the virtual environment is no longer active

By default, *mkvirtualenv* uses the system default *Python* interpreter. If a
specific version of python is required, it can be specified with the -p
parameter:

.. code-block:: bash

    $ mkvirtualenv -p python2 (name)    # Python2 virtual environment
    $ mkvirtualenv -p python3.4 (name)  # Python3.4 virtual environment

When entering a new virtual environment, there will be no access to the
software libraries previously installed on the system, as the purpose of this
tool is to have an isolated environment. To add a new library to the virtual
environment, you only have to install it using *pip* (remember to previously
activate the environment), and the same rule works for other pip commands like
uninstall or freeze:

.. code-block:: bash

    (uvispace) $ pip install <library-name>

Finally, certain projects, like *uvispace*, have predefined libraries and
versions and they are specified under a *requirements.txt* file (The name does
not have to be necessary the same). In this case, these libraries can be
automatically installed on the virtual environment using the following command:

.. code-block:: bash

    (uvispace) $ pip install -r requirements.txt

Other useful commands are the following:

.. code-block:: bash

    $ lsvirtualenv        # List all available virtual environments
    $ rmvirtualenv (name) # Deletes a virtual environment

A more extensive reference on the *virtualenvwrapper* utility can be found on
its `documentation <https://virtualenvwrapper.readthedocs.io>`_.

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
  It is used when none of the previous interfaces can be used. It is tipically
  used to export signals outside of *Qsys* and represent the particular
  interface of some chip. It is also used to connect two componnets with custom
  user-defined interface between them.

Using the standard buses makes the connection between components fast because
*Qsys* "knows" their signals and its meaning. In a single click a memory mapped
slave can be connected to a processor, saving time and avoiding errors.
*SOPC-Builder* and *Qsys* create a lot of logic automatically. For example,
*Avalon MM* buses create the logic circuits needed to implement the desired
address map. *SOPC-Builder* and *Qsys* also have a big library of IP
(Intelectual Property) cores that are tested and documented. This permits the
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

..  image:: /_static/QuartusCompilation.png
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
*[project folder]/software* as the project workspace, being not neccessary to
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

..  image:: /_static/FPGA_Software_EDS.png
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
  the FPGA is loaded into the non-volatile EPCS memory availables in the board.
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
      to recompile it genrate the BSP and recompile the BSP and software
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

In this scenario few more attemps to directly from SOPC Builder to Qsys can be
done. If there is no success the best option is to start from bottom a new
project and go adding functionality to it. First the Nios II and its memories.
If it works, Ethernet capabilities can be added, and so on. The error probably
is in some files missing for the Ethernet controller or some files missing in
one of the multiple IP cores added with Quartus and the IP Wizzard. This Wizard
IPs usually come with some files to configure some tables of data needed to do
the functionality of the IP. If this is the case the compiler can compile
leaving that memories empty and later the IP core functionality is wrong.
