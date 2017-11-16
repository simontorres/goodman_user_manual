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
   Here you have two options, one with ``iraf`` and one without it.

   - Standard: Without Iraf (Python 2 or 3).

     ``conda create -n astroconda python=2.7 stsci``

    or

     ``conda create -n astroconda python=3 stsci``

     *astroconda* is the name of your environment, you can use any name you want.

   - Legacy software stack: Iraf included (Requires Python 2.7).

     ``conda create -n astroconda python=2.7 iraf-all pyraf-all stsci``

5. Activate your environment.

   ``source activate astroconda``

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

