Getting Started
===============

This section covers the first steps for downloading the project and setting
up a PC in order to be able to collaborate developing and testing the project.

..  toctree::
    :maxdepth: 2

    Getting Started         <starting>

System requirements
-------------------

The software project has been tested correctly in the following OS:

- **Ubuntu 14.04 LTS (Trusty)**

Besides, the following libraries and tools needs to be installed for running
correctly the project:

- **OpenCV**: open-source library compatible with *Python* and *C++*. It offers
  lots of image processing functions with plenty of documentation. In the
  current version, it was used the version *OpenCV2*, though there is a more
  recent one (*OpenCV3*). `<http://opencv.org/>`_
- **scikit-image**: is an open-source *SciPy* third party library for dealing
  with image processing operations. In this project,  it is used for image
  segmentation and shapes classification. It can be obtained more information
  at `<http://scikit-image.org/>`_.
- **PySerial**: Python library used for communicating through a serial port
  with external devices. An overview and download information can be obtained
  at `<https://pythonhosted.org/pyserial/pyserial.html>`_.
- **Sphinx**: this library is an excellent tool for creating intelligent Python
  documentation. Indeed this documentation was generated using it. It can access
  the *docstrings* inside the project and allow to automatically update the
  documentation when they are changed. It allows to create HMTL or PDF documents
  from plain text (*ReStructured text*) using a variety of templates or
  customize a new one. There is a *Getting Started* tutorial at the
  `official webpage <http://www.sphinx-doc.org/en/1.5.1/index.html>`_.
  Moreover, it has been used *ReadTheDocs* for hosting the documentation. It
  offers tools for compiling the code and generating the HTML files in their
  own server. `<https://readthedocs.org/>`_.

.. _download-source:

Download the source
-------------------

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

Learning
--------

Python
^^^^^^

Python is the programming language used for the most of the Software project.
It has a vast community, and there are lots of places to learn about it. Prior
to developing code for the *UviSpace* project, it is highly recommended to learn
basic programming ideas relating to Python. Here, we refer to some books
covering this knowledge:

* *Learn Python the Hard Way* [1]_ is a book oriented to the learning of the
  Python language through a large amount of proposed exercises. Moreover, there
  is free version that can be obtained online. A link is provided in the
  *Bibliography* section.
* *Think Python* [2]_ is another book for Python programming beginners. As the
  previous one, it is free to read online and download. Just follow the link
  provided in the *Bibliography*.

Moreover, a reader-friendly Python program is much more valuable than a mess of
*spaghetti code*, and we encourage the readers and future developers of the
*UviSpace* project to learn good ways of writing Python code, if they haven't
yet:

* *The PEP 8* [3]_ is a Python Enhancement Proposal(PEP), namely a style guide
  proposed by the Python creator and its community. It is not too long, and the
  benefits are really high, thus it is a highly recommended reading. There are
  more PEPs published in the same webpage, and it worths to take a look at some
  of them.
* *Writing Idiomatic Python* [4]_ is a book aimed to Python programmers with a
  prior knowledge, and is not recommended to use it as a first approach to the
  Python programming. Anyway, it proposes several coding techniques that improve
  the code readability with little effort

git
^^^

The project uses *git* for doing the versioning control of the project. There
is a practical guide book in their webpage [5]_

It is recommended to read at least the first 2 chapters for learning the basic
working of *Git*.

Regarding the versions naming convention, it has been followed the *Semantic
Versioning 2.0.0* [6]_. In its web there is a detailed explanation about the
rules for assigning a tag to a new version.

|

.. rubric:: **Bibliography**

.. [1] Learn Python the Hard Way, 3rd edition (2014) `<https://learnpythonthehardway.org/book/>`_
.. [2] Think Python, 1st edition (2012) `<http://greenteapress.com/wp/think-python/>`_
.. [3] PEP 8 -- Style Guide for Python Code `<https://www.python.org/dev/peps/pep-0008/>`_
.. [4] Writing Idiomatic Python `<https://jeffknupp.com/writing-idiomatic-python-ebook>`_
.. [5] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_
.. [6] Semantic Versioning 2.0.0 `<http://semver.org/>`_
.. [7] Mobile Robots: Navigation, Control and Remote Sensing (2011) `<http://onlinelibrary.wiley.com/book/10.1002/9781118026403>`_

|
