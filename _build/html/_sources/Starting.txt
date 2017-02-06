

Getting Started
===============

This section covers the first steps for downloading the project and setting 
up a PC in order to be able to collaborate developing and testing the project.


System requirements
-------------------

The software project has been tested correctly in the following OS:

- **Ubuntu 14.04 LTS (Trusty)**

Besides, the following libraries and tools needs to be installed for running
correctly the project:

- **ROS**: It is a set of libraries and tools used for developing robot oriented software in *Python* and *C/C++*. Each ROS version is associated to a concrete Ubuntu distribution, and it is highly recommended to respect this correspondence. Thus, for *Ubuntu 14.05*, the corresponding ROS version is ROS Indigo. Information and installation instructions can be obtained at `<http://wiki.ros.org/indigo>`_.

Download the source
-------------------

The project is stored and mantained at an online repository that can be accesed `here <https://github.com/jlrandulfe/UviSpace>`_. 
The project can be directly downloaded from the repository page. However, it is recommended to `install git <https://git-scm.com/downloads>`_ and then clone the project to a local repository. This way, you can use the *git version control system* in order to collaborate to the project and easily synchronize your work with the other developers.

Once you have git installed and configured, type the following instructions for cloning the UviSpace project to your PC::

    cd /go/to/desired/directory                             #Set the terminal working directory
    git clone https://github.com/jlrandulfe/UviSpace.git    #Clone the project to your local machine
    
After that, the project will be cloned to a folder named UviSpace into the directory you have specified.

Learning
--------

git
===

The project uses *git* for doing a versioning control of the project. There 
is a practical guide in [1]_

.. rubric:: **Bibliography**

.. [1]  Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_