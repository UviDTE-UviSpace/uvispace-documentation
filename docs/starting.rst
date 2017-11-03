Getting Started
===============

This section covers the first steps for downloading the project and setting
up a PC in order to be able to collaborate developing and testing the project.

..  toctree::
    :maxdepth: 2

    Getting Started         <starting>

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

git
^^^

The project uses *git* for doing the version control of the project. There
is a practical guide book in their webpage [1]_. It is recommended to read
at least the first 2 chapters for learning the basic working of *Git*.

In order to practice with test repositories, GitHub provides a web tutorial [2]_
which helps to solidify knowledge of git terms and operation.

Regarding the versions naming convention, it has been followed the *Semantic
Versioning 2.0.0* [3]_. In its web there is a detailed explanation about the
rules for assigning a tag to a new version.

|

.. rubric:: **Bibliography**

.. [1] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_
.. [2] Try Git `<https://try.github.io>`_
.. [3] Semantic Versioning 2.0.0 `<http://semver.org/>`_

|
