.. _`Installing Pipeline`:

Apendix A: Installation Instructions
####################################

We strongly recommend installing the pipeline using *virtual environments*.
Below you will find a summary of installation steps.

.. warning:: Remember that we are not providing any kind of support for
  installation. This documentation will be the only existing.

The following list provides a summary of all the steps.

- Install ``anaconda``
- Add ``astroconda`` channel
- Create virtual environment
- Activate environment
- Install requirements
- Install pipeline

Anaconda and Virtual Environment
********************************

For anaconda installation we recommend you to check the `astroconda channel's
documentation page <https://astroconda.readthedocs.io>`_. The instructions will
be reproduced here but they might change for newer versions. Also they are
limited to the *best case scenario*.

.. warning:: Anaconda installer requieres BASH. Don't try with other shell.

1. Installing ``anaconda``
   - Go to https://www.anaconda.com/downloads and download the appropriate
   *anaconda installer* for your platform, most likely it has been
   automatically selected.

   - Run the installer.

     ``cd <download_directory>``

     ``bash <install_script>``

   - Once completed, check the bottom of ``~/.bash_profile`` or  ``~/.bashrc``
     there should be a new ``PATH`` definition with anaconda included.

2. Check ``anaconda`` installation

   ``which conda``

   You should get a response similar to this:

   ``~/bin/anaconda3/bin/conda``

   If you don't get this response check the detailed instructions on the
   astroconda site. Otherwise continue to the next step.

3. Configure Conda to use the *Astroconda Channel*

   ``conda config --add channels http://ssb.stsci.edu/astroconda``

4. Create a virtual environment.
   We have dropped support for python2.7 so you have to use >3.5.

     ``conda create -n astroconda python=3 stsci``

     *astroconda* is the name of your environment, you can use any name you want.

5. Activate your environment.

   ``source activate astroconda``

Goodman Spectroscopic Pipeline
******************************

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

.. _`Install DCR`:

Install DCR
***********

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
^^^^^^^^^^^^^

Compiling ``dcr`` is actually very simple.

  ``cd <path_to_download_location>/goodman/pipeline/data/dcr-source/dcr/``

Then simply type:

  ``make``

This will compile `dcr` and also it will create other files. The executable
binary here is ``dcr``.


I have successfully compiled *dcr* in several platforms, such as:

1. Ubuntu 16.04
2. Centos 7.1, 7.4
3. MacOS Sierra
4. Solaris 11


Install binary DCR
^^^^^^^^^^^^^^^^^^

This is a suggested method. If you are not so sure what you are doing, we recommend
you following this suggestion. If you are more advanced user you just need the
``dcr`` executable binary in your ``$PATH`` variable.


1. Open a terminal
2. In your home directory create a hidden directory ``.bin`` (Home directory
   should be the default when you open a new terminal window)

   ``mkdir .bin``

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

