Getting Started
===============

This section covers the first steps for running UviSpace and collaborate
with it.

..  toctree::
    :maxdepth: 3

    Getting Started         <starting>


How to Use Uvispace
-------------------

Hardware Setup
^^^^^^^^^^^^^^
To run Uvispace it is needed:

- To install A CPU-based platform (PC or embedded board) that can run the main
  controller. Instructions on how to setup the main controller are given in
  `here
  <https://uvispace-main-controller.readthedocs.io/en/latest/starting.html>`_.

- To install the localization system. It is formed by four FPSoC boards and four
  CMOS cameras. The compatible hardware to build it and setup instructions
  are given `here <https://uvispace.readthedocs.io/en/latest/camera.html>`_.

- To build one or more UGVs. The compatible chassis, motors, extra PCBs and
  wiring for the UviSpace UGVs is available
  `here <https://uvispace.readthedocs.io/en/latest/vehicle.html>`_.

All the elements of the system are expected to live in the same Network. A
router will be needed to:

- Localization nodes have fixed MAC and ask the IP via DHCP. A DHCP table must
  be built in the router in order cameras to get IP. The IPs of the cameras
  given by the router must match those used in the main controller to access
  them (`uvispace-main-controller
  <https://github.com/UviDTE-UviSpace/uvispace-main-controller>`_/uvispace/uvisensor/resources/config/video_sensor<num>.cfg).

- The UGVs admit different protocols:

  - Zigbee: (FINISH THIS). In this case a USB Zigbee Emitter needs to be
    connected to the Main Controller.

  - Wifi: The Wifi-to-Serial module in the UGVs that use Wi-Fi must be
    configured with fixed IP. The router must be Wireless and
    generate a SSID with a password that matches that configured
    in the Wifi-to-Serial module.

  The type of UGV implemented (chassis), communications type (Zigbee or Wifi)
  and communication adderesses of the built UGV must match with the
  (`uvispace-main-controller
  <https://github.com/UviDTE-UviSpace/uvispace-main-controller>`_/uvispace/ubirobot/resources/config/robot<num>.cfg).
  There is one configuration file of this kind per robot. Vehicles are
  numered with natural numbers starting in 1. This number is also used
  to identify them in the software. When the user runs uvispace through
  console the UGV number is passed and this is how the software knows
  which configuration file to load. It is recommended to identify the
  UGV with its number. In University of Vigo UGVs are identified with a
  number next to the identification marker, like shown in the following
  figure.

  ..  image:: /_static/starting-figs/ugv-with-number.png
      :width: 200px
      :align: center

- The Main Controller can be connected through wire to the router or wirelessly
  (if the router is Wireless) to the rest of the elements. In this case the
  IP of the main controller is not important and can be assigned dinamically
  by the DHCP The WPT system (wireless charging is still not fully )

.. note::  Details (MACs, IPs, etc.) for the network configuration for the
           UviSpace implementation in University of Vigo can be found inside
           the Google Drive account dte.uvispace@gmail.com.

Download the source
^^^^^^^^^^^^^^^^^^^

The project source code is stored in the repositories available under the
`UviSpace organization <https://github.com/UviDTE-UviSpace>`_.
These repositories allow to download the source code directly, but it is
recommended to `install git <https://git-scm.com/downloads>`_ and then clone
the project to a local repository. This way, you can use the *git version
control system* in order to collaborate with the project and easily synchronize
your work with the rest of developers.

The active repositories of the UviSpace project are:

- `uvispace-main-controller <https://github.com/UviDTE-UviSpace/uvispace-main-controller>`_:
  It includes all the software and documentation for the main controller.

- `uvispace-documentation <https://github.com/UviDTE-UviSpace/uvispace-documentation>`_:
  It includes de documentation of the UviSpace controller (this website) except
  the documentation of the main controller.

- `uvispace-camera-fpga <https://github.com/UviDTE-UviSpace/uvispace-camera-fpga>`_:
  It includes the hardware for the FPGA of the FPSoC boards used in the
  localization nodes.

- `uvispace-camera-hps <https://github.com/UviDTE-UviSpace/uvispace-camera-hps>`_:
  It includes the software for the processor of the FPSoC boards used in the
  localization nodes.

- `pcb-designs <https://github.com/UviDTE-UviSpace/pcb-designs>`_:
  It contains the PCBs for the UGV, a schematic with the wiring of the UGVs
  and the PCBs of the WPT system.

- `arduino-UGV-controller <https://github.com/UviDTE-UviSpace/arduino-UGV-controller>`_:
  It contains the code for the Arduino board controlling the UGV.

- `3D-designs <https://github.com/UviDTE-UviSpace/3D-designs>`_: It stores
  3D designs of the Uvispace project.

The rest of repositories are deprecated versions of the UviSpace software
and should not be used.


Run Uvispace
^^^^^^^^^^^^

The following videos show the different capabilities of the UviSpace project:

**Run UviSpace from console**: The user can specify a set of points using the
console. The UGV consecutively passes these points and stops in the last.

(MAKE VIDEO)

**Run UviSpace from GUI**: The user can specify a set of points using the
through a .csv file or interacting with the GUI.

(MAKE VIDEO)

**Run UGV Messenger**: It permits to interact with the vehicle and check if the
communications are properly working. The battery level of the UGV is printed
in the console every second. Useful for debugging purposes.

(MAKE VIDEO)

**Get video or images from camera**: In the resources folder of the uvisensor
package there are programs to easily interact with the UviSpace
localization nodes and extract images and video. Useful for debugging
purposes.

(MAKE VIDEO)

**Run Teleoperation**: The Teleoperation program permits to control the
UGV with the keyboard arrows like a remote control car. Useful for debugging
the UGV communications and Arduino software.

(MAKE VIDEO)

How to collaborate with Uvispace
--------------------------------

Git Workflow
^^^^^^^^^^^^

To collaborate with UviSpace first a GitHub administrator must give permission
to the developer to all or some of the repositories in the `UviSpace
organization <https://github.com/UviDTE-UviSpace>`_.

Then the developer must download the repositories that will be used into a
local address in the computer. It is desired that a the repositories the
designer is using have the following branches:

- master: always works and matches the online documentation.

- develop: derived from master. It is used by all the developers of this
  project to merge their branches and test the code before merging with
  the master.

- developer_main_branch: derived from develop. It is tipically used by a
  developer to design a new feature for the program. In example: if a new neural
  controller for the UGVs is to be designed by a developer a branch
  called i.e. "neural_controller" must be created for this purpose. Each
  developer usually has a main branch where he/she works in a feature.

- developer_local_branches: derived from developer_main_branch. The developer
  can create sporadic branches locally to test some risky tasks he/she does
  not know will work. If the code in this branch does finally not work the
  developer can come back to the developer_main_brach just erasing the
  new branch he created. master, develop and developer_main_branch can be
  synchronized with GitHub. However, in order to mantain a small number
  of branches online and not mess the repository it is prefered that
  developer_local_branches are mantained locally in the developers computer
  and not synchronized online.

Commits should be created when some partial task is finished and should
have small and concise messages (i.e. "Modify main README file",
"Add Start Button to the Main Window", etc.). It is encouraged not to mix
files from different tasks in the same commit (i.e. "Modify main README file
and Add Start Button to the Main Window"). This will make the developer
to commit between one time every 2 days to 2 times per day approximately.

When all developers finish their features and are ready to mix their codes
their one developer should merge the main branches of the others with
his/hers in the develop branch and fix the conflicts that appear (two developers
touched the same line of the same file). This can e done locally.

Once the conflicts are solved and the develop branch tested it can be
merged to master. To merge with the master it is encouraged to use pull
requests from GitHub. It is encouraged to have at least one reviewer
of for the Pull request (ideally two), that check the commits generated
in the develop branch before merging with the master. This will
improve the quality of the written software.

Once the code is merged to master the unused branched must be erased from
GitHub as soon as possible.

The developer should use the GitHub Issues to write down the different bugs
and possible improvements for the project. This way the other developers are
informed and can work on that issue when they are idle.


Uvispace Style Rules
^^^^^^^^^^^^^^^^^^^^

When collaborating with the UviSpace project Follow these rules to ensure high
quality:

- Always use english language. This includes variable names and comments in the
  code and the documentation.

- Use small variable names and the most concise as possible.

- When programming, placing the comments over the statement is preferred to
  adding them next to the statement. That is::

      # This way of commenting is preferred
      a = b.copy()

      a = b.copy() # Use this kind of commenting as little as possible

- For all programming languages use only 80 characters per line. This ensures
  easy reading of the code in all kind of screens and GitHub. You can configure
  the text editor to draw a line at 8 characters to help you with that.

- Use 4 space indentatation for Python and other languages. You can program
  your text editor to put 4 spaces every time you press tab. This will make the
  code more readable when switching from one text editor to another
  one.

- The GitHub repositories are always named with lowercase leters using hyphens
  to separate the words. Example: example-repository-name. Try to name the
  internal folders of the repository following the same rule.

- The comments in the Python code of the main controller are used by the
  autodoc extension of Sphinx to autogenerate its documentation. This is very
  useful because it permits to comment the code and generate the documentation
  website for the project at the same time. To know the synthax rules of
  autodoc check the code of the main controller or visit [1]_.


UviSpace Documentation
^^^^^^^^^^^^^^^^^^^^^^

The documentation is generated with Sphinx (a Python library) and stored
in `<https://readthedocs.org/>`_. The documentation is divided in two websites:

- `<https://uvispace.readthedocs.io/en/latest/>`_:
  It is the main documentation of Uvispace (this website). It contains the
  documentation of all UviSpace parts except for the main controller. Its source
  code and instructions on how to use Sphinx to compile the documentation are
  given in the repository
  `<https://github.com/UviDTE-UviSpace/uvispace-documentation>`_.

- `<https://uvispace-main-controller.readthedocs.io/en/latest/>`_.
  It is the documentation of the main controller. Its source code is stored in
  the repository of the main controller:
  `<https://github.com/UviDTE-UviSpace/uvispace-main-controller>`_. This
  documentation is separated from the main documentation because it has a
  different Sphinx configuration. The documentation for the main controller
  uses the autoc extension of the Sphinx package to build the documentation
  of the code from docstrings (special comments) located in the source code.
  In this way the documentation for the code and the website are automatically
  generated.

The GitHub repositories and websites are linked using GitHub webhooks that
update the websites every time a commit is pushed to the repositories.
Therefore modifying the source code of the documentation the websites
are automatically generated.

The text in the README files of the repositories should be kept at minimum.
Only two informations are allowed in the README files:

- Small description of the repository of folder contents.
- Compilation and running instructions for the code stored in the repository.
  This can help a user working in a repository to copy and paste common
  commands he/she will use when working with the repository (so it is not
  needed to access to the bigger readthedocs documentation). The information
  in the READMEs should be thought as a cheatsheet and should be also explained
  in the readthedocs documentation.

**IMPORTANT**: The content of the **readthedocs documentation websites** must be
ALWAYS consistent with the code in the **master** branches of the repositories.
Therefore the code developed in some other branches should not be merged to the
master until its documentation is also prepared. Once the documentation
and the code are finished in branches different to master they can be merged to
the master branches simultaneously (at least the same day).

.. note::  The documentation in the readthedocs websites contains generic
           explanations of the UviSpace Project. Details for the implementation
           in the University of Vigo are stored in the Google account
           dte.uvispace@gmail.com. The following Google services are used:

           - `Drive <https://www.google.com/intl/en/drive/>`_ :
             It contains details on the network configuration (MACs, IPs, etc.),
             pictures (useful for preparing final grade project reports), videos
             and test results.

           - `Youtube <https://www.youtube.com/channel/UCPiWvkxKKlAubP5u4L0pxpg?view_as=subscriber>`_:
             It contains public and private videos about how to run UviSpace and
             videos testing the project.

Learning
--------

This section collects learning sources to help getting started programming:

Git
^^^

The project uses *git* for doing the version control of the project. There
is a practical guide book in their webpage [2]_. It is recommended to read
at least the first 2 chapters for learning the basic working of *Git*.

In order to practice with test repositories, GitHub provides a web tutorial [3]_
which helps to solidify knowledge of git terms and operation.

Regarding the versions naming convention, it has been followed the *Semantic
Versioning 2.0.0* [4]_. In its web there is a detailed explanation about the
rules for assigning a tag to a new version.

Python
^^^^^^
Python is used to program the main controller and some of the code in the
localization nodes. Python is also used to generate the documentation
with Sphinx. You can find Python learning resources in the
Getting Started page of the `Main Controller Documentation
<https://uvispace-main-controller.readthedocs.io/en/latest/>`_.

C/C++
^^^^^^
C is used to program the drivers in the localization node. C++ is used to
program some auxiliary applications in the localization node.

The basic book for learning C is [5]_, written by its inventors Brian W.
Kernighan and Dennis M. Ritchie. Some people do not like this book and prefer
more modern resources like [6]_, an interative tutorial.

For C++ a good website with deep explanations and a lot of examples is
[7]_. An iterative tutorial for C++ can be found in [8]_.


FPSoC
^^^^^^
FPSoCs are devices that contain processors and reconfigurable logic in the
form of Field Programmable Gate Array (FPGA) in the same chip. The FPSoC
family Cyclone V SoC, from Intel FPGA, is used to implement to implement the
UviSpace localization nodes. The Electronic Technology Department of University
of Vigo hosts studies and programs related to FPSoC platforms in the GitHub
organization `UviDTE-FPSoC <https://github.com/UviDTE-FPSoC>`_. A designer
that wants to modify the code of the UviSpace localization nodes is
encouraged to visit the following repositories of that organization:

- `CycloneVSoC-examples <https://github.com/UviDTE-FPSoC/CycloneVSoC-examples>`_:
  It contains a starting guide to Cyclone V SoC, tutorials for compilation of
  different OSs, instructions to run baremetal (no OS) applications and a set of
  small examples mostly related to transfer data between processor and FPGA.
- `CycloneVSoC-time-measurements <https://github.com/UviDTE-FPSoC/CycloneVSoC-time-measurements>`_:
  It contains code and complete numeric results for a characterization of the
  processor-FPGA communications in Cyclone V SoC. Reading the main README in this
  repository can help a designer to understand the inner workings of the device
  and help him decide which communications resources are more appropiate for a
  certain task.

UviSpace Tutorials
^^^^^^^^^^^^^^^^^^
The tutorials developed during the development of UviSpace project are stored
in the `Tutorials page of the Uvispace documentation
<https://uvispace.readthedocs.io/en/latest/tutorials.html>`_. To be coherent and
place all Python-related learning resources, Python Tutorials are stored
in the `Tutorials page of the Uvispace main controller documentation
<https://uvispace-main-controller.readthedocs.io/en/latest/tutorials.html>`_.




|

.. rubric:: **Bibliography**

.. [1] Sphinx Autodoc Website `<http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_
.. [2] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_
.. [3] Try Git `<https://try.github.io>`_
.. [4] Semantic Versioning 2.0.0 `<http://semver.org/>`_
.. [5] The C Programming Language Second Edition by Brian W. Kernighan and Dennis M. Ritchie.
       `<http://www.dipmat.univpm.it/~demeio/public/the_c_programming_language_2.pdf>`_.
.. [6] Learn C Interactive Tutorial `<https://www.learn-c.org/>`_
.. [7] Cplusplus Tutorial `<http://www.cplusplus.com/doc/tutorial/>`_
.. [8] Learn C++ Interactive Tutorial `<https://www.learn-cpp.org/>`_

|
