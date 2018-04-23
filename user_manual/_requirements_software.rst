Software Requirements
*********************
Using the pipeline remotely is the recommended method, in which case you don't need
to worry about software requirements.

However, we provide simple instructions below.


Setup for Remote Use
^^^^^^^^^^^^^^^^^^^^
The Goodman Spectroscopic Data Reduction Pipeline has been installed on a
dedicated computer at SOAR. The procedure requires to open a VNC session, for which
you need to be connected to the SOAR VPN. The credentials for the VPN are the
same you used for your observing run, provided by your *Support Scientist*, who
will also give you the information for the data reduction computer VNC
connection.

.. note:: IRAF is available in all three data servers. Running ``iraf`` will
    open an *xgterm* and *ds9* windows. ``iraf-only`` will open *xgterm* but
    not *ds9*

Establish a VNC connection
~~~~~~~~~~~~~~~~~~~~~~~~~~
Separately, you should receive a server hostname, IP, display number and
VNC-password. If you don't you can ask for it. We have decided to use a similar
organization of vnc displays as for ``soaric7``:

.. _`VNC Displays table`:
.. table:: VNC display number and working folder assigned to each partner.

   ========= ===================== ====================================
    Display    Partner/Institution     Folder
   ========= ===================== ====================================
       :1      NOAO                  ``/home/goodman/data/NOAO``
       :2      Brazil                ``/home/goodman/data/BRAZIL``
       :3      UNC                   ``/home/goodman/data/UNC``
       :4      MSU                   ``/home/goodman/data/MSU``
       :5      Chile                 ``/home/goodman/data/CHILE``
   ========= ===================== ====================================

For this tutorial we will call the vnc server host name as ``<vnc-server>``
the display number  is ``<display-number>`` and your password is ``<password>``.

We are not recommending a particular *VNC Client* since there are several options,
so if you feel that you are not getting the best image quality feel free to
explore different clients. For this tutorial we use ``vncviewer``.

VNC from the Terminal
~~~~~~~~~~~~~~~~~~~~~
Find the ``<display-number>`` that corresponds to you from the `VNC Displays table`_.
Open a terminal, and assuming you have installed ``vncviewer``.

    ``vncviewer <vnc-server>:<display-number>``

You will be asked to type in the ``<password>`` provided.

.. important::

    The real values for ``<vnc-server>`` and ``<password>``
    should be provided by your support scientist.

If the connection succeeds you will see a *Centos 7* Desktop using *Gnome*.

Setup for local installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
6. Get latest release of the *Goodman Spectroscopic Pipeline*

   visit https://github.com/soar-telescope/goodman/releases/latest and download
   the ``*.zip`` or ``*.tar.gz``

   ``cd <download_location>``

   ``tar -xvf <pipeline_file>.tar.gz``

   or

   ``unzip <pipeline_file>.zip``


7. Install requirements from ``requirements.txt``

   ``cd <goodman_pipeline_unpacked_location>``

   ``pip install -r requirements.txt``

8. Install the pipeline

   ``pip install .``

9. Upgrading the pipeline

   ``pip install . --upgrade``

DCR (optional)
~~~~~~~~~~~~~~
.. warning:: Don't forget to cite: Pych, W., 2004, PASP, 116, 148

In terms of cosmic ray rejection we shifted to a non-python package because the
results were much better compared to LACosmic's implementation in astropy.
LACosmic was not designed to work with spectroscopy though.

The latest version of the Goodman Spectroscopic Pipeline uses a modified version
of ``dcr`` to help with the pipeline's workflow. It is included under

  ``<path_to_download_location>/goodman/pipeline/data/dcr-source/dcr/``

``goodman`` is the folder that will be created once you untar or unzip the latest
release of the *Goodman Spectroscopic Pipeline*.

.. important::

    The changes includes deletion of all ``HISTORY`` and ``COMMENT`` keywords,
    which we don't use in the pipeline. And addition of a couple of custom
    keywords, such as: ``GSP_FNAM``, which stores the name of the file being
    created. ``GSP_DCRR`` which stores the reference to the paper to cite.


You are still encouraged to visit the official  `Link <http://users.camk.edu.pl/pych/DCR/>`_
own by the author and let me remind you once more that you have to cite the
paper mentioned several times in this manual.

Compiling DCR
-------------

Compiling ``dcr`` is actually very simple.

  ``cd <path_to_download_location>/goodman/pipeline/data/dcr-source/dcr/``

Then simply type:

  ``make``

This will compile `dcr` and also it will create other files. The executable
binary here is ``dcr``.


We have successfully compiled *dcr* in several platforms, such as:

- Ubuntu 16.04
- Centos 7.1, 7.4
- MacOS Sierra
- Solaris 11


Install binary DCR
------------------

This is a suggested method. If you are not so sure what you are doing, we recommend
you following this suggestion. If you are a more advanced user you just need the
``dcr`` executable binary in your ``$PATH`` variable.


1. Open a terminal
2. In your home directory create a hidden directory ``.bin`` (Home directory
   should be the default when you open a new terminal window)

   ``mkdir ~/.bin``

3. Move the binary of your choice and rename it ``dcr``. If you compiled it,
   most likely it's already called ``dcr`` so you can ignore the renaming part of
   this step.

   ``mv dcr.Ubuntu16.04 ~/.bin/dcr``

   Or

   ``mv dcr ~/.bin/dcr``

4. Add your ``$HOME/.bin`` directory to your ``$PATH`` variable. Open the file
   ``.bashrc`` and add the following line.

   ``export PATH=$PATH:/home/myusername/.bin``

   Where ``/home/myusername`` is of course your home directory.

5. Close and reopen the terminal or load the ``.bashrc`` file.

    ``source ~/.bashrc``



System Installation (not recommended)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
System installation is not recommended because can mess up things specially in Mac OS.
If you are really committed to install the pipeline in your system we recommend the `Conda Installation`_

6. Get latest release of the |pipeline full name|

   visit https://github.com/soar-telescope/goodman/releases/latest and download
   the ``*.zip`` or ``*.tar.gz`` file.

   ``cd <download_location>``

   ``tar -xvf goodman-<version>.tar.gz``

   or

   ``unzip goodman-<version>.zip``


7. Install requirements from ``requirements.txt``

   ``cd goodman-<version>``

   ``pip install -r requirements.txt``

8. Install the pipeline

   ``pip install .``

9. Upgrading the pipeline

   ``pip install . --upgrade``


Conda Installation
~~~~~~~~~~~~~~~~~~

We strongly recommend installing the pipeline using *virtual environments*.
Below you will find a summary of installation steps.

.. warning:: Remember that we are not providing any kind of support for
  installation. After this documentation you are on your own.

The following list provides a summary of all the steps (follow the links for instructions).

- `Install Anaconda <https://conda.io/docs/user-guide/install/index.html>`_
- `Add astroconda channel <https://astroconda.readthedocs.io/en/latest/installation.html#configure-astroconda-channel>`_
- `Create virtual environment`_
- Activate environment
- Install requirements
- Install pipeline

.. _`create virtual environment`:
New Virtual Environment
-----------------------
Creating virtual environments is well documented on the `Conda documentation site <https://conda.io/docs/user-guide/tasks/manage-environments.html>`_
just make sure you are using ``Python 3.5`` or ``3.6``. which are the versions
against |pipeline name| is regularly tested.

Existing Virtual Environment
----------------------------
We provide a predefined environment through a ``environment.yml`` file that you
can use to create a virtual environment with all the pipeline's dependencies.
It goes as follows:

  ``conda create -f environment.yml``

The new environment will be called ``goodman``.

Pipeline Installation
---------------------

Finally, in order to install |pipeline name| using a virtual environment you need
to activate it first.

  ``source activate goodman``

And in case you used a different name replace ``goodman`` by the name of your environment.

In a terminal go to ``<path to download>/goodman-<version>/``, then:

  ``python setup.py test``

If all the tests run successfully  you can then install the pipeline with:

  ``python setup.py install``

