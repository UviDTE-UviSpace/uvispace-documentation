

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
- **OpenCV**: open-source library compatible with *Python* and *C++*. It offers lots of image processing functions with plenty of documentation. In the current version, it was used the version *OpenCV2*, though there is a more recent one (*OpenCV3*). `<http://opencv.org/>`_
- **scikit-image**: is an open-source *SciPy* third party library for dealing with image processing operations. In this project,  it is used for image segmentation and shapes classification. It can be obtained more information at `<http://scikit-image.org/>`_.
- **Sphinx**: This library is an excellent tool for creating intelligent Python documentation. Indeed this documentation was generated using it. It can access the *docstrings* inside the project and allow to automatically update the documentation when they are changed. It allows to create HMTL or PDF documents from plain text (*ReStructured text*) using a variety of templates or customize a new one. There is a *Getting Started* tutorial at the `official webpage <http://www.sphinx-doc.org/en/1.5.1/index.html>`_. Moreover, it has been used *ReadTheDocs* for hosting the documentation. It offers tools for compiling the code and generating the HTML files in their own server. `<https://readthedocs.org/>`_.

Download the source
-------------------

The project is stored and maintained at an online repository that can be accessed `here <https://github.com/jlrandulfe/UviSpace>`_. 
The project can be directly downloaded from the repository page. However, it is recommended to `install git <https://git-scm.com/downloads>`_ and then clone the project to a local repository. This way, you can use the *git version control system* in order to collaborate to the project and easily synchronize your work with the other developers.

Once you have git installed and configured, type the following instructions for cloning the UviSpace project to your PC::

    $ cd /go/to/desired/directory                             #Set the terminal working directory
    $ git clone https://github.com/jlrandulfe/UviSpace.git    #Clone the project to your local machine
    
After that, the project will be cloned to a folder named UviSpace into the directory you have specified.

Learning
--------

Python
^^^^^^

Python is the programming language used for the most of the Software project. It has a vast community, and there are lots of places to learn about it. Prior to developing code for the *UviSpace* project, it is highly recommended to learn basic programming ideas relating to Python. Here, we refer to some books covering this knowledge:

 * [1]_ is a book oriented to the learning of the Python language through a large amount of proposed exercises. Moreover, there is free version that can be obtained online. A link is provided in the *Bibliography* section.

Moreover, a reader-friendly Python program is much more valuable than a mess of *spaghetti code*, and we encourage the readers and future developers of the *UviSpace* project to learn good ways of writing Python code, if they haven't yet:

* [2]_ is a Python Enhancement Proposal(PEP), namely a style guide proposed by the creators and the community of Python. It is not too long, and the benefits are really high, thus it is a highly recommended reading. There are more PEPs published in the same webpage, and it worths to take a look at some of them.
* [3]_ is a book aimed to Python programmers with a prior knowledge, and is not recommended to use it as a first approach to the Python programming. Anyway, it proposes several coding techniques that improve the code readability with little effort

git
^^^

The project uses *git* for doing the versioning control of the project. There 
is a practical guide in [4]_

It is recommended to read at least the first 2 chapters for learning the basic working of *Git*. 

Regarding the versions naming convention, it has been followed the *Semantic Versioning 2.0.0* In [5]_ there is a detailed explanation about the rules for assigning a tag to a new version.

|

.. rubric:: **Bibliography**

.. [1] Learn Python the Hard Way, 3rd edition (2014) `<https://learnpythonthehardway.org/book/>`_
.. [2] PEP 8 -- Style Guide for Python Code `<https://www.python.org/dev/peps/pep-0008/>`_
.. [3] Writing Idiomatic Python `<https://jeffknupp.com/writing-idiomatic-python-ebook>`_
.. [4] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_
.. [5] Semantic Versioning 2.0.0 `<http://semver.org/>`_

|