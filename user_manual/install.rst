.. _Install:

Installation Instructions
#########################

We strongly recommend installing the pipeline using *virtual environments*.
Below you will find a summary of installation steps.

.. warning:: Remember that we are not providing any kind of support for
  installation. This documentation will be the only existing.

- Install ``anaconda``
- Add ``astroconda`` channel
- Create virtual environment
- Activate environment
- Install requirements
- Install pipeline

Anaconda
********

For anaconda installation we recommend you to check the `astroconda channel's
documentation page <https://astroconda.readthedocs.io>`_. The instructions will
be reproduced here but they might change for newer versions.

- Get Anaconda Installer:


Install DCR
***********

In terms of cosmic ray rejection we shifted to a non-python package because the
results were way better compared to LACosmic's implementation in astropy.
LACosmic was not designed to work with spectroscopy though.

Visit this `Link <http://users.camk.edu.pl/pych/DCR/>`_ to download the code and
find the instructions for compiling. I have added a few pre-compiled binaries
and if you are lucky they will work right away. The available binaries are
located in ``goodman/dcr`` and the options are:

  - dcr.Ubuntu16.04
  - dcr.Centos7
  - dcr.MacOSSierra
  - dcr.Solaris11


Choose whatever version fits your needs and rename it ``dcr`` and put it in a
folder that at the same time is in your ``$PATH`` variable. If you don't know
what that is follow the next section.

Install binary DCR
^^^^^^^^^^^^^^^^^^

1. Open a terminal
2. In your home directory create a hidden directory ``.bin`` (Home directory
   should be the default when you open a new terminal window)

   ``mkdir .bin``

3. Move the binary of your choice and rename it ``dcr``. If you compiled it
   most likey it's already called ``dcr`` so you can ignore this step.

   ``mv dcr.Ubuntu16.04 ~/.bin/dcr``

4. Add your ``$HOME/.bin`` directory to your ``$PATH`` variable. Open the file
   ``.bashrc`` and add the following line.

   ``export PATH=$PATH:/home/myusername/.bin``

   Where ``/home/myusername`` is of course your home directory.

5. Close and reopen the terminal or load the ``.bashrc`` file.

    ``source ~/.bashrc``

