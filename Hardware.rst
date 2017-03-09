
Hardware System Design
======================

..  toctree::
    :maxdepth: 2

    Hardware system design        <Hardware>

FPGA System
-----------

The system implemented in this Project is depicted in the figure below. The biggest part of the system is a *NiosII* processor system defined in *SOPC-Builder*. The rest of the system is defined directly in *Quartus II*.

..  image:: /_static/fpga_diagram.png
    :width: 750px
    :align: center

SOPC-Builder system
^^^^^^^^^^^^^^^^^^^

- **Nios II processor:** soft-processor controlling all the hardware components. It deals with the initialization of all components and the communication through Ethernet. Communications permit remote control of the system via commands. The instruction bus and data bus of the processor are connected to both SDRAM and Flash memory, so both this memories can be used as data and program memory.

- **SRAM Controller:** It controls the 2MB volatile SRAM available in the board. When debugging using the *ios II EDS* it is used as program and data memory for the *Nios II* processor. When the development is finished and the program for the processor is loaded into the non-volatile flash memory, the SRAM memory is only the data memory for the system.

- **Flash Controller:** It controls the 8MB Flash non-volatile memory available in the board. When debugging using the *Nios II EDS* it can be used to store the MAC address of the board or any other configuration/calibration data. When the program is finished and the board does not need the computer anymore the Flash memory is used as program memory to store the program for the *Nios II* processor.

- **PLL:** It generates the clock signals for the system from a 50MHz source. *C0* and *C2* are used inside *SOPC-Builder*. *C0* to *C4* are exported outside *SOPC-Builder* for being used by the *Quartus II* components (orange part). Some of the clock signals generated in *SOPC-Builder* and exported are not used in *Quartus II* side because there is another PLL there. This PLL should be removed and the system should use the clocks generated in the main PLL inside Qsys. 

- **Avalon Camera:** It provides a register-based interface for the configuration of the *Camera Controller* component from the *Nios II* processor.

- **APIO Sensor:** It provides a register-based interface for the configuration of the *Sensor Controller* component from the *Nios II* processor. 

- **Clock-crossing I/O:** It connects buses with different clock signals. In this case it connects the secondary *Avalon MM data bus* (sourced by *C2*) with  the main *Avalon MM data bus* (sourced by *C0*). 

- **JTAG UART:** It permits to communicate *Nios II EDS*  with the Nios II, to use the *Nios II* console, to load the programs in the *SDRAM* and to do step-by-step debugging. It has an interrupt line connected to the *Nios II* with the highest priority 0 (lower number means higher priority).

- **Ethernet controller:** It controls the *PHYS Ethernet* chip in the board that implements the physical layer of the TCP/IP protocol. It can use *Direct Memory Access* (DMA) and read and write directly to the *SRAM* memory (data memory). Its interrupt has the lowest priority in the system (number 2).

- **Sys_ID:** This component stores a number identifying the system and a time stamp when the *Nios II* system was generated in *SOPC-Builder*. The *ios II EDS* checks these two values before loading a *Nios II* program to ensure that the system connected is the correct target for the software project (these 2 numbers are passed in the *sopc_info* file used to generate the software projects).

- **Timer:** It is the timer used by the *Real Time Operating System* (RTOS) running in the *Nios II* for scheduling its tasks. After the JTAG UART this timer should have (and has) the highest priority interrupt (number 1).

- **Tracker0 to Tracker 5** 

- **LED PIO:** GPIO peripheral to control from *Nios II* the green LEDs 0 to 7 in the board.

- **LCD Controller:** Used to control the LCD screen from Nios II. It is used by the software to print some information like the IP address.

Quartus II part
^^^^^^^^^^^^^^^

- **Camera Controller:** It is in charge of the camera configuration, RAW data capture from the camera and data conversion from RAW to RGB and grayscale. There are 4 buses of 10 bits each: one for each RGB channel and one more for gray scale channel. 

- **Sensor Controller:** it implements the image processing for the detection of robots marked by red triangles, using therefore only the red channel from the Camera Controller. The output of the algorithm are the x and y coordinates of the 4 points defining the square that encloses each triangle. *X* and *Y* are 12 bit buses and are sent to the trackers. Another output is the RGB and gray images that are stored to SDRAM memory through the SDRAM controller.

- **SDRAM Controller:** it controls the 128MB SDRAM memory in the board.

Besides the previous components, there are also present in the hardware design several LEDs, which show the programmer useful information: 

- Red LED 17
- *RED LED 16*, set to ON when the *Camera Controller* is providing a video output.
- *RED LED 15*, set to ON when the input of the *VGA controller* is connected to the output of of the *SDRAM controller*. If the *Camera Controller* is providing an image it should appear on the Screen connected to the VGA port.
- RED LED 14: set to ON when the processor orders the camera to acquire images.
- RED LED 13
- RED LED 12
- RED LED 11

FPGA Hardware compilation  and file organization
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following steps show how to set up the FPGA project:

- **Edit the Nios II System using the SOPC-Builder.**

- **Save and Generate the SOPC-Builder project**. After that, several files will be created in the project folder:

    - *DE2_115_SOPC.sopcinfo:* XML file containing the structure and settings for the components in the *Nios II* System. This file is used by *Nios II EDS* to know which hardware is inn the system and ease *Nios II* programming.
    - *DE2_115_SOPC.v:* Main HDL file for the *Nios II* system, that instantiates all the IP cores that were previously added in the *SOPC-Builder*.
    - The rest of HDL files for Nios II system are *.v* files copied into the project folder. They are the source files of all of the components added in *SOPC-Builder*. Altera library IP cores (like Flash Controller, SRAM Controller, LCD Controller, Timer, Sys_ID) are copied from the Altera installation folder. The rest of IP cores (Avalon Camera, APIO Sensor, Trackers and Ethernet Controller) are copied from  folders inside the project folder (*ip/Avalon_camera, ip/apio_sensor, ip/Avalon_locator* and *ip/eth_ocm*).

- **Instantiate the Nios II System and other components in Quartus II**. The *Nios II* system, represented now by *DE2_115_SOPC.v* is a component and can be used in *Quartus II* designs using *DE2_115_SOPC* as component name. To use the *Nios II* system and connect it to other components, the file  *DE2_115_WEB_SERVER.v* is created. It defines the main entity for the FPGA hardware project. The name of this entity should be the same name as the project name because Quartus II starts the compilation looking for the entity with the same name as the project. Inside the script, the *Nios II* System (DE2_115_SOPC) is instantiated along with the other components in the *Quartus II* part of the system (orange part of the diagram), i.e. *Camera controller, Sensor controller,  SDRAM Controller, VGA Controller* and *Display Controller*. The source files for these components are in subfolders *camera, display* and *sensor* inside the project folder. In *DE2_115_WEB_SERVER.v* the components are connected to the FPGA pins using names (LEDR, CLOCK_50, etc.). The relation between these names and the actual pins on the chip is in the *DE2_115_WEB_SERVER.qsf*. This file can be generated for any DE2-115 board using the System Builder program in the DE2-115 CD-ROM documentation. 

- ** Add compilation files to Quartus II**. All HDL files used in the project should be added to the files tab in Quartus II (left upper part of the program), so the compiler can find them. The following files should be added: 

    - *DE2_115_WEB_SERVER.v:* It is the main file containing the highest level entity.
    - *DE2_115_SOPC.v:* This file contains the definition of the *Nios II* system.
    - *DE2_115_SOPC.quip:* This file imports the rest of the files which describe the components used in *SOPC-Builder* that form the *Nios II* System. *.quip* files are used for importing a group of HDL files together. 
    - The rest of *.quip* files for the *Quartus II* part of the system (orange part in the diagram), namely *display/vga.quip, display/display.quip, camera/camera.quip, etc.*

- **Add libraries**. the source files of *eth_ocm* component are added as a library. Then, add it in *Quartus II->Assignments->Settings->Libraries*.

- **Compile the project**. Press *compile design* or *Processing-> Start Compilation*. Firstly, *Analysis&Synthesis* converts HDL design to the FPGA hardware. The, the *Fitter* does the *Place & Route* of the circuit (locates the circuit in the specific device assigned to the project and interconnects all the elements). Finally, the *Assembler* generates the *.sof* file used to program the FPGA, and the program should look similar to the figure below.

..  image:: /_static/QuartusCompilation.png
    :width: 750px
    :align: center

FPGA Software
-------------

As stated in the Introduction, the software project is formed by 2 subprojects: *SocketServer*, that is the application project developed for implementing the desired functionality; and *Socket_Server_bsp*, which defines the hardware architecture of the system that is being used.

Import a software project
^^^^^^^^^^^^^^^^^^^^^^^^^

Usually, when one starts a new project both projects are generated at the same time using *File->New Nios II Application and BSP From Template*. In this case, the projects are already created so they have to be imported into the workspace. To do this go to *File->Import->General->Existing Projects* then go to *workspace->Select root directory [project folder]/software* and click *copy projects into workspace*. Another recommended way is to choose *[project folder]/software* as workspace and do not copy the projects into workspace.

Change sopc_info file location in the BSP
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The BSP uses global location to find the *.sopc_info* file. When changing the location of the folder, the path of the *.sopc_info* file has to be changed before generating the BSP again. It can be done in the *settings.bsp* file inside the BSP project under the *<SopcDesignFile>* entry.

Software project compilation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The steps required to compile the system are the following:

- **Generate BSP:** Right click at *Socket_Server_bsp->Nios II->Genrate BSP*.
- **Compile BSP:** Right clickat  *Socket_Server_bsp->Build*.
- **Compile application:** Right click at *SocketServer-> Build*. The makefile combines the BSP and application compilations to generate a single output *.elf* file located inside the application folder. In this case it is called *SocketServer.elf*. This file is the program for the *Nios II* processor.

In the figure below, it can be observed how will look the EDS after opening the application and BSP projects.

..  image:: /_static/FPGA_Software_EDS.png
    :width: 750px
    :align: center

MAC and IP
^^^^^^^^^^

The used MAC and IP settings by the program are contained in the *MAC_IP_config.h* file. 

- **Using DHCP:** If USEDHCP macro is defined in the file, the board will ask for an IP address via DHCP using the MAC specified inside the file. If DHCP fails (after 2 minutes) the board sets the IP address written in the file.
- **Fixed IP:** If USEDHCP macro is NOT defined in the file (i.e. the line commented), the MAC and IP used will be the ones specified in the file, and DHCP is not used.

How to load hardware and software in the FPGA board
---------------------------------------------------

There are 2 different options:

- **While debugging:** In this case the board is connected to the PC for debugging. The console in the *Nios II EDS* can be used. In this situation both the program and data of the *Nios II* processor are stored in the SRAM memory. The DE2_115_sopcbuilder12 design accomplish this task:

    - In the *Nios II* processor component in *SOPC-Builder* reset and exception vectors have to point to the *SRAM Controller* component (so the SRAM becomes its program memory).
    - Compile the SOPC Builder project and compile using Quartus II.
    - Set the RUN-PROG switch in RUN position and switch on the board.
    - Load the DE2_115_WEB_SERVER.sof file into the FPGA using the Quartus Programmer in JTAG mode.
    - With the BSP pointing to the last version of the DE2_115_SOPC.sopcinfo Genrate the BSP and recompile the BSP and software projects.
    - Load the SocketServer.elf into the SRAM memory doing: Nios II EDS->Right click SocketServer->Run as->Nios II Hardware. The first time you load a .elf you have to configure the loading processes using : Right click SocketServer->Run configurations. There the board can be detected. Uncheck Ignore mismatched System ID or ignore mismatched system timestamp if you are sure that .elf is targeting the board connected (sometimes it fails even the software project is generated from the .sopc_file of the system in the board).
    - The program is loaded and it starts. You can use the console as std input and output.

- **Stand-alone board:** (THIS STEPS DON'T WORK) In this case the program of the Nios II is loaded into the non-volatile flash memory and the configuration of the FPGA is loaded into the non-volatile EPCS memory availables in the board. When the board is configured using this option, during the start-up the FPGA is loaded from the EPCS and the Nios II starts reading its program from the flash memory, that is now its program memory. The SRAM memory is one more time the data memory of the processor. This is the mode used to put the cameras in the ceiling of the lab. We provide this version in the DE2_115_sopcbuilder12_flash folder. The steps to do that:

    - Delete old version of DE2_115_sopcbuilder12_flash.
    - Take the DE2_115_sopcbuilder12 project and copy it, change the name to the copy by DE2_115_sopcbuilder12_flash.
    - In the Nios II processor component in SOPC Builder set the reset and exception vectors pointing to the Flash controller component (now the Flash component is the program memory of the system).
    - Compile the SOPC Builder project and compile using Quartus II.
    - Convert the DE2_115_WEB_SERVER.sof file into DE2_115_WEB_SERVER.pof file using Quartus II->File->Convert Programming file. It is generated in the project folder. Choose EPCS64 as device and Active programming as settings, and DE2_11_WEB_SERVER.sof as input file.
    - Set the RUN-PROG switch in PROG position and switch on the board.
    - Load the DE2_115_WEB_SERVER.pof file into the EPCS memory using Quartus II programmer in Active-Serial mode. 
    - Switch off the board, switch the RUN-PROG switch to RUN position and switch on the board again. Now the FPGA is configured from the EPCS.
    - The previous .elf used in debug mode can be used. However, if you need to recompile it genrate the BSP and recompile the BSP and software projects again.
    - In the Nios II EDS right click in SocketServer project->Nios II->Flash programmer. Click File->New->Get Flash programmer system from SOPC Information File->Select the .sopc_info file generated in the last compilation (the one where Nios II reset and exception vectors are pointing to the flash memory) ->click OK, add the SocketServer .elf file and press start. The .elf file should be downloaded into the flash.


Migrate the hardware project to Qsys
------------------------------------

An attempt to migrate this code to Qsys was done. Using Qsys instead of SOPC Builder permits to use the newer versions of Quartus like Quartus II. Otherwise old Quartus versions have to be used. The last version with SOPC Builder is the Quartus II 12.1. This version contains both Qsys and SOPC Builder and permits the migration from SOPC Builder to Qsys.

Several attempts were done to migrate the design to Qsys without success.

- First the less number of modifications to the design were done. The design was open with Qsys and migrated to Qsys. The SOPC Builder files are automatically sent to a Back Up. In this process Qsys erases all .quip files in Files Tab of Quartus II. Qsys saves all data in DE115_SOPC/systhesis folder. The DE115_SOPC/systhesis folder DE115_SOPC.qip is added to Quartus. The rest of .qip files for the components added in Quartus II (like sensor or camera) are added too. The design compiles. The processor works but Ethernet do not. Some file is probably missing in the compilation. The compilation process does not break but the design is not fully completed.  

- Starting from zero. The design was  open with Qsys, regenerated. All files in Files tab in Quartus II are removed. In this scenario when the compilation crashes because files are not found, they are supplied one by one. The project compiles but the end result seems worse than before because the program cannot be loaded into the Nios II processor.

In this scenario few more attemps to directly from SOPC Builder to Qsys can be done. If there is no success the best option is to start from bottom a new project and go adding functionality to it. First the Nios II and its memories. If it works, Ethernet capabilities can be added, and so on. The error probably is in some files missing for the Ethernet controller or some files missing in one of the multiple IP cores added with Quartus and the IP Wizzard. This Wizzard IPs usually come with some files to configure some tables of data needed to do the functionality of the IP. If this is the case the compiler can compile leaving that memories empty and later the IP core functionality is wrong. 
