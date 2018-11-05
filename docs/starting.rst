Getting Started
===============

This section covers the first steps for running UviSpace and collaborate
with it.

..  toctree::
    :maxdepth: 3

    Getting Started         <starting>


How to Use Uvispace
-------------------

Download the source
^^^^^^^^^^^^^^^^^^^

The project source code is stored in the repositories available under the
`UviSpace organization <https://github.com/UviDTE-UviSpace>`_.
These repositories allow to download the source code directly, but it is
recommended to `install git <https://git-scm.com/downloads>`_ and then clone
the project to a local repository. This way, you can use the *git version
control system* in order to collaborate with the project and easily synchronize
your work with the rest of developers.

For example, once you have git installed and configured, type the following instructions
for cloning the UviSpace Main Controller source code to your PC::

    $ cd /go/to/desired/directory                                             # Set the terminal working directory
    $ git clone https://github.com/UviDTE-UviSpace/uvispace-main-controller   # Clone the project to your local machine

After that, the main controller will be cloned to a folder named uvispace-main-controller into the directory you have specified.

(TALK ABOUT THE DIFFERENT REPOSITORIES)


Install Uvispace
^^^^^^^^^^^^^^^^


Run Uvispace
^^^^^^^^^^^^
(To be done when the UGV is running, maybe make a video)


How to collaborate with Uvispace
--------------------------------

Workflow
^^^^^^^^
Talk about branches
master branch
remove branches as soon as possible.
commit everyday
small commit messages
merge to master
pull requests
issues


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

All repos are documented here less the main controller that is documented here.
This is done because this repo has different sphinx configuration that permits
autodocumenting of the python code using comments in the text.

As low as possible as README in the repos. Most of documentation unified here.
The content in repos are content of the repo to help navigate and quick guide
with the commands.

The documentation in this website and main controller should be consistent
with the content in the master branch of each repository.

Features must be developed in different branches and merged to master when finished.
All code added must update the corresponding documentation.

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

.. [1] Sphinx Autodoc Website <http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_
.. [2] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_
.. [3] Try Git `<https://try.github.io>`_
.. [4] Semantic Versioning 2.0.0 `<http://semver.org/>`_
.. [5] The C Programming Language Second Edition by Brian W. Kernighan and Dennis M. Ritchie.
       `<http://www.dipmat.univpm.it/~demeio/public/the_c_programming_language_2.pdf>`_.
.. [6] Learn C Interactive Tutorial `<https://www.learn-c.org/>`_
.. [7] Cplusplus Tutorial `<http://www.cplusplus.com/doc/tutorial/>`_
.. [8] Learn C++ Interactive Tutorial `<https://www.learn-cpp.org/>`_

|
