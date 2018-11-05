.. UviSpace documentation master file, created by
   sphinx-quickstart on Wed Oct 26 11:31:59 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

..  image:: /_static/cover1.jpg
    :width: 1100px
    :align: center

UviSpace
========

UviSpace is a free software testbed developed in the *Electronic Technology
Department* in the `University of Vigo <http://uvigo.gal/uvigo_en/index.html>`_
to remotely control electric Unmanned Ground
Vehicles (UGVs) in indoor spaces. A group of camera-based localization nodes
placed in the ceiling of the laboratory is in charge of detecting geometric
shapes attached to the top of the vehicles. This information is transmitted
to a main controller that extracts the pose (position and orientation) of the
UGVs and sends the appropiate linear and angular speed setpoints to their motors
depending on the trajectory to be followed.

The main goal of UviSpace is to provide a friendly, affordable
and easy to mantain environment, where works related to a variety of topics
can be carried out, such as distributed control, neural network controllers,
or battery management systems, to name just a few.  The multidisciplinarity of
the working environment is also very suitable for students to carry out different
types of projects.


Source Code
===========

.. image:: /_static/github-logo.png
    :target: https://github.com/UviDTE-UviSpace
    :width: 250px
    :align: left
    :alt: link to UviSpace GitHub organization

The project's source code is available on the repositories of our
`GitHub organization <https://github.com/UviDTE-UviSpace>`_. Anybody is welcome
to clone and work with them. If you wish to collaborate, just contact us
(contact information in the Introduction page).

|

Documentation
=============

..  toctree::
    :maxdepth: 1

    Introduction            <introduction>
    Getting Started         <starting>
    Main Controller         <main-controller>
    Localization system     <camera>
    UGVs                    <vehicle>
    Wireless Power Transfer <wpt>
    Tutorials               <tutorials>
    Release Notes           <release-notes>
