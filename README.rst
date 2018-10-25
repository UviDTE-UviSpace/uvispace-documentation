========
UviSpace
========

:Author:
    Department of Electronic Technology,

    University of Vigo

:Version: 1.0.0

=====================
Project documentation
=====================

.. image:: https://readthedocs.org/projects/uvispace/badge/?version=latest
   :target: http://uvispace.readthedocs.io/en/latest/?badge=latest
   :alt: Documentation Build Status

The oficial documentation about the UviSpace project is hosted at `this website
<http://uvispace.readthedocs.io/en/latest/>`_.

====================
Create documentation
====================

| :ref:`1. Install Python in Windows <python-windows>`
| :ref:`2. Install the Virtual Environment <virtual-env>`
| :ref:`3. Create a new virtual environment <new-virtual-env>`
| :ref:`4. Install Sphinx <sphinx>`
| :ref:`5. Create documentation <documentation>`


Before creating documentation of a project, few steps have to be done. We'll
show in this tutorial how to install important tools to have an organized and
methodical way of creating a project documentation. All the following is going
to be done using the Windows command promt **cmd**.

.. _python-windows:

1 - Install Python in Windows
--------------------------------

The first tool that has to be installed is Python. Python is the programming
language that Uvispace uses. It uses the version 2.

Python can be downloaded `here <https://www.python.org/downloads/>`_.

* For Windows 32-bit just click on the Download Python button (version 2).
* For Windows 64-bit click on *Windows* in the question "Looking for Python
  with a different OS?" and download the *Windows x86-64 MSI installer* of
  python.

During the installation process take care that the option *installing pip* is
checked. Pip is a package management system used to install and manage software
packages written in Python.

.. _virtual-env:

2 - Install the Virtual Environment
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

   $ pip install virtualenvwrapper-win

If you also have Python 3 installed you can force installation in Python 2
doing:

.. code-block:: bash

   $ py -2.7 -m pip install virtualenvwrapper-win


.. _new-virtual-env:

3 - Create a new virtual environment
--------------------------------------

To create a new virtual environment for an specific project, run the following
command:

.. code-block:: bash

   $ mkvirtualenv name

Note that "name" has to be sustituted by the virtual environment name. We
usually use 'uvispace' as name.

If you also have Python 3 installed you can force installation in Python 2
doing:

.. code-block:: bash

   $ mkvirtualenv -p <path-to-python2-executable> name

`path-to-python2-executable` is uausally equal to: C:\Python27\python.exe.

Finally, before installing the project libraries, enter in the new virtual
environment so they are installed inside the virtual environment folder.

.. code-block:: bash

   $ workon name

.. _sphinx:

4 - Install Sphinx
---------------------

Sphinx is a tool to generate documentation. Once in the virtual environment of
the project run the following command:

.. code-block:: bash

   $ pip install sphinx

.. _documentation:

5 - Create documentation
--------------------------

First of all, it is needed to clone the repository uvispace-documentation.
Now, using GitHub and the git bash, create a new branch on the repository in
order to not affect the rest of the files.

Inside the docs folder of the uvispace documentation repository, there are some
rst files (ReStructured Tex). They can be opened with `atom <https://atom.io/>`_
or you can create new ones in that directory. By the way, Sphinx have to be run
in order to generate the documentation to view the final results.

Once the repository cloned, in the Windows command promt (inside the proper
virtual environment), go to the folder that contains the repository
'..\uvispace-documentation' and get into 'docs'.
In order to run Sphink you have to run the make.bat file from the virtual
environment in witch is installed Sphinx.
To do that run the following command:

.. code-block:: bash

   $ make html

Now, the Sphinx has generated a html file where you can see the final result.
Those html files are located in the ..\\docs\\_build\\html.
