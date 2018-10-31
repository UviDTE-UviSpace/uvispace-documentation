Getting Started
===============

This section covers the first steps for downloading the project and setting
up a PC in order to be able to collaborate developing and testing the project.

..  toctree::
    :maxdepth: 3

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


Uvispace Style Rules
--------------------

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


C/C++
^^^^


FPSoC
^^^^^^

UviSpace Tutorials
^^^^^^^^^^^^^^^^^^

How to run Uvispace
-------------------
(To be done when the UGV is running, maybe make a video)


UviSpace Documentation
----------------------

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



|

.. rubric:: **Bibliography**

.. [1] Sphinx Autodoc Website <http://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_
.. [2] Pro Git book, 2nd edition (2014). `<https://git-scm.com/book/en/v2>`_
.. [3] Try Git `<https://try.github.io>`_
.. [4] Semantic Versioning 2.0.0 `<http://semver.org/>`_

|
