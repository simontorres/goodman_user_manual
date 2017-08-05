.. _Install:

Installation Instructions
#########################

Installation will slightly depend on the system but in general is simple and can
be sumarized in the following steps:

- Download_ the pipeline code
- Install prerequisites_
- :ref:`Install <general-installation>` the pipeline

It is very important to note that this pipeline has been developed using
*Python 2.7*, although we have done it in a way that would allow a smooth
transition to *Python 3.5* we haven't tested it neither are we planning to do it
in the near future.

.. _prerequisites:
Install Dependencies
********************
There are two types of dependencies that have to be met. The system prerequisite
installation depends on the platform itself so they will be detailed bellow in
their respective subsection.

The python libraries are specified in the file ``requirements.txt`` and
installing them is very easy. But it has to be done later on.

.. _`ubuntu install`:
Ubuntu 16.04
^^^^^^^^^^^^

Some other python-specific tools, if you already run python code most likely you
already have them.

    ``sudo apt-get install python-setuptools python-dev build-essential``

    ``sudo easy_install pip``

We have decided to use Qt4Agg backend since Qt seems to be the most multi
platform compatible backend.

    ``sudo apt-get install python-qt4``

.. _`centos install`:
CentOS 7
^^^^^^^^

Start by installing the ``EPEL repository``

    ``sudo yum -y install epel-release``

Update the database with

    ``sudo yum -y update``

This takes a while...

Install pip

    ``sudo yum -y install python-pip``

Upgrade pip

    ``sudo pip install --upgrade pip``

Install python-devel

    ``sudo yum install python-devel``


.. _`macos install`:
Installing on MacOSX
^^^^^^^^^^^^^^^^^^^^

Although I have succesfully run the pipeline in two separated MacOS X machines
the installation process was a bit tricky and has not been fully tested. I plan
to update this section in the next weeks.

.. _`virtuenvinstall`:
Install Using Virtual Environments
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Virtual Environment installation has not been fully tested.

.. _Download:
Downloading the Goodman HTS Spectroscopic Pipeline
**************************************************
In order to get the code for the pipeline there a many options, our suggestion
is to download the official release ``tar.gz`` file

https://github.com/soar-telescope/goodman/blob/master/dist/goodman-1.0b1.tar.gz

.. _`general-installation`:
Installing the Pipeline
***********************

First of all install the python requirements. Your location must be the same as
the file ``requirements.txt`` which should be your recently cloned repository

    ``sudo pip install -r requirements.txt``

Once this has succeeded proceed to install the pipeline using:

    ``sudo python setup.py install --record files.txt``

This will install the pipeline in your system and also will create a file
``files.txt`` that contains the list of files created at installation time and
will be very helpfull if you ever want to fully remove the pipeline.


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

