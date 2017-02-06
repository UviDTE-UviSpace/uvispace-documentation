

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

Python
^^^^^^

Python is the programming language used for the most of the Software project. It has a vast community, and there are lots of places to learn about it. Prior to developing code for the *UviSpace* project, it is highly recommended to learn basic programming ideas relating to Python. Here, we refer to some books covering this knowledge:

 * [1]_ is a book oriented to the learning of the Python language through a large amount of proposed exercises. Moreover, there is free version that can be obtained online. A link is provided in the *Bibliography* section.

Moreover, a reader-friendly Python program is much more valuable than a mess of *spaghetti code*, and we encourage the readers and future developers of the *UviSpace* project to learn good ways of writing Python code:

* [2]_ is a Python Enhancement Proposal(PEP), namely a style guide proposed by the creators and the community of Python. It is not too long, and the benefits are really high, thus it is a highly recommended reading.
* [3]_ is a book aimed to Python programmers with a prior knowledge, and is not recommended to use it as a first approach to the Python programming. Anyway, it proposes a bunch of coding techniques that improve the code readability with little effort

git
^^^

The project uses *git* for doing a versioning control of the project. There 
is a practical guide in [4]_

It is recommended to read at least the first 2 chapters for learning the basic working of *Git*.


.. rubric:: **Bibliography**

.. [1] Learn Python the Hard Way, 3rd edition (2014) `<https://learnpythonthehardway.org/book/>`_
.. [2] PEP 8 -- Style Guide for Python Code `<https://www.python.org/dev/peps/pep-0008/>`_
.. [3] Writing Idiomatic Python `<https://jeffknupp.com/writing-idiomatic-python-ebook>`_
.. [4] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_