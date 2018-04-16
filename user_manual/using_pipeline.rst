General Considerations on using the pipeline
############################################
The Goodman Spectroscopic Pipeline is meant to work as a single package.
However, the full process is split in two separate modules: ``redccd`` and
``redspec``. The first does the basic 2D image reduction, applying bias, flat
field corrections, and cosmic ray removal. The second module, ``redspec``, takes
the corrected 2D images output by ``redccd`` and produces wavelength-calibrated
1D spectra.

The pipeline is run from the command line in a terminal window. Each module is
run separately, first ``redccd`` followed by ``redspec``, however, you could
run both sequentially from e.g. a shell script, just make sure you move to the
the right directory.

In order to make things easier you should organize your data:

1. Make sure all the data in your folder corresponds to the same binning,
   readout mode, region of interest (ROI), and grating/wavelength mode
   combination.
2. You should have bias, flats (quartz or dome flats), and the appropriate
   comparison lamps. Other files like acquisition images, slit images and focus
   images should be deleted.
3. Do not mix dome flats with quartz lamp flats. As an example, suppose you took
   both quartz lamps and dome flats for your targets. You could create two folders,
   one with the science data and the dome flats, and another with the same
   science data and the quartz lamps. Then, if you run the pipeline in each
   folder you can compare the results and decide which type of flat works best
   for my particular case.

Command line arguments
**********************
For a list of the options and command line arguments type ``--help`` argument:


For ``redccd``

.. literalinclude:: files/redccd_help.txt

And for ``redspec``

.. literalinclude:: files/redspec_help.txt

.. raw:: pdf

    PageBreak

Lists of Reference Lamps Available
**********************************

The automatic wavelength calibration relies on having previously calibrated
reference lamps obtained in the same configuration or mode. It is also important
that the lamp names are correct, for instance ``HgAr`` is quite different than
``HgArNe``. For interactive wavelength calibration, reference lamps are used as
a visual aid only. It lets you find the matching laboratory lines values that
will be used to fit a pixel to wavelength relation that we call
*Wavelength Solution* The list of lamps is the following.

   +---------+------+--------+--------+
   | Grating | Mode | Filter |  Lamp  |
   +=========+======+========+========+
   |   400   |  M1  | None   | HgAr   |
   +---------+------+--------+--------+
   |   400   |  M1  | None   | HgArNe |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | Ar     |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | Ne     |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | HgAr   |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | HgArNe |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | CuHeAr |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | FeHeAr |
   +---------+------+--------+--------+


.. important::

    More lamps will be made public shortly.



..   | 600-old | Blue | None   | HgAr   |
   +---------+------+--------+--------+
   | 600-old | Blue | None   | CuHeAr |
   +---------+------+--------+--------+
   |  1200   |  M2  | None   | CuHeAr |
   +---------+------+--------+--------+
   |  1200   |  M3  | None   | CuHeAr |
   +---------+------+--------+--------+
   |  1200   |  M5  | GG455  | CuHeAr |
   +---------+------+--------+--------+


Adding new reference lamps
**************************

It is possible to add new lamps, you just need a raw lamp that meets
the following specifications with respect to your science project:

   - Same instrument configuration or mode
   - Same Grating
   - Same order blocking filter if present
   - Same binning
   - Same lamp/combination that you use in your observations
   - Smallest slit possible. Equal is OK too.


See `Creating Reference Lamps`_ for instructions on how to proceed.
Now you have two options, identify the system folder where the lamps that come
with the package are saved and simply put it there or put it in another
directory and use the argument ``--reference-files``

   ``redspec --reference-files /path/to/ref-lamp-location``

If you requiere that your lamps are made available as a part of the package you
can contact ``storres [at] ctio [dot] noao [dot] edu`` and we will handle your
request.

.. _`Header Requirements`:

Headers Requirements
^^^^^^^^^^^^^^^^^^^^

Goodman HTS spectra have small non-linearities on their wavelength solutions.
They are small but big enough that they MUST be taken into account.

It was necessary to  implement a custom way of storing non-linear wavelength
solutions that at the same time allowed for keeping data as *untouched* as
possible. The main reason is that linearizing the reference lamps made
harder to track down those non-linearities on the new data being calibrated and
also; The documentation on how to write non-linear solution to a FITS header is
not good, besides it appears that nobody is trying to improve it neither
trying to implement it. Below I compile a list of requiered keywords for
comparison lamps if they want to be used as reference lamps. The full list of
keywords is listed under Headers_.

General Custom Keywords:

  Every image processed with the *Goodman Spectroscopic Pipeline* will have the
  general custom keywords. The one required for a reference lamp is the following:

    ``GSP_FNAM = file-name.fits // Current file name``


Record of line centers in Pixel and Angstrom:

  Every line detected in the reference lamp is recorded both in its pixel value
  and later (most likely entered by hand) in angstrom value. The root string is
  ``GSP_P`` followed by a zero-padded three digit sequential number
  (001, 002, etc). For instance.

    ``GSP_P001= 499.5377036976768  / Line location in pixel value``

    ``GSP_P002= 810.5548319623747  / Line location in pixel value``

    ``GSP_P003= 831.6984711087946  / Line location in pixel value``

  The equivalent values in angstrom are then recorded with the root string
  ``GSP_A`` and the same numerical pattern as before.

    ``GSP_A001= 5460.75            / Line location in angstrom value``

    ``GSP_A002= 5769.61            / Line location in angstrom value``

    ``GSP_A003= 5790.67            / Line location in angstrom value``


  ``GSP_P001`` and ``GSP_A001`` are a match. If any of the angstrom value entries
  have a value of ``0`` (default value) the equivalent pixel entry is ignored.

.. important::

  Those keywords are used to calculate the mathematical fit of the
  wavelength solution and are not used on normal operation. Our philosophy here
  is that the line identification has to be done only once and then the
  model can be fitted several time, actually you can try several models
  if you want.

Non-linear wavelength solution:

  The method for recording the non-linear wavelength solution is actually
  very simple. It requires: ``GSP_FUNC`` which stores a string with the name of
  the mathematical model from ``astropy.modeling.models``. ``GSP_ORDR`` stores
  the order or degree of the model. ``GSP_NPIX`` stores the number of pixels in
  the spectral axis. Then there is N+1 parameter keywords where N is the order
  of the model defined by ``GSP_ORDR``. The root string of the keyword is ``GSP_C``
  and the rest is a zero-padded three digit number starting on zero to N.
  See the example below.

    ``GSP_FUNC= Chebyshev1D          / Mathematical model of non-linearized data``

    ``GSP_ORDR= 3                    / Mathematical model order``

    ``GSP_NPIX= 4060                 / Number of Pixels``

    ``GSP_C000= 4963.910057577853    / Value of parameter c0``

    ``GSP_C001= 0.9943952599223119   / Value of parameter c1``

    ``GSP_C002= 5.59241584012648e-08 / Value of parameter c2``

    ``GSP_C003= -1.2283411678846e-10 / Value of parameter c3``

.. warning::

    This method has been developed and tested to write correctly polynomial-like
    models. And ONLY reads ``Chebyshev1D`` models.
    Other models will just be ignored. More development will be done based on
    request, suggestions or needs.


.. _`Creating Reference Lamps`:

Creating Your Own Reference Lamps
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you use a Custom mode you will need a Custom reference lamp too. You are welcome
to try to do it, just make sure the `Header Requirements`_ are met. If not
we might be able to help, depending on time availability, it will help a lot if
you have the lines identified already.

.. important::

    We are not ready to offer a simple tool to construct the reference lamps.


.. include:: headers.rst

.. raw:: pdf

    PageBreak

.. _`Running Pipeline`:

Running the pipeline in the SOAR data reduction computer
########################################################

The Goodman Spectroscopic Data Reduction Pipeline has been installed on a
dedicated computer at SOAR. The procedure is to open a VNC session, for which
you need to be connected to the SOAR VPN. The credentials for the VPN are the
same you used for your observing run, provided by your *Support Scientist*, who
will also give you the information for the data reduction computer VNC
connection.

.. note:: IRAF is available in all three data servers. Running ``iraf`` will
    open an *xgterm* and *ds9* windows. ``iraf-only`` will not open *ds9*

Establish a VNC connection
**************************
Separately, you should receive a server hostname, IP, display number and
VNC-password. If you don't you can ask for it. We have decided to use a similar
organization of vnc displays as for ``soaric7``:

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

For the rest of this tutorial we will assume your host name is ``vnc-server``
the display number  is ``1`` and your password is ``password``.
Though we recommend using RealVNC, most other VNC clients will work fine (e.g.,
Remmina in Linux). For GNU/Linux and Mac OSX machines we suggest the RealVNC
Viewer client. For Windows machines, we suggest either the RealVNC Viewer client
or the UltraVNC viewer client.
We also know that Vinagre and vncviewer on GNU/Linux work fine.

VNC from the Terminal
^^^^^^^^^^^^^^^^^^^^^
Open a terminal, and assuming you have installed ``vncviewer``.

    ``vncviewer vnc-server:1``

You will be asked to type in the *password* provided.

VNC using a Graphical Client
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Using a graphical VNC client is quite similar and intuitive

.. image:: img/realvnc1.png
    :width: 1300px
.. image:: img/realvnc_login.png
    :width: 800px

In this case the *IP address* was used, which is equivalent and sometimes
better.

Dealing with Virtual Environments
*********************************

The Goodman Spectroscopic Pipeline uses virtual environments since they allow
for easy portability and also give a higher level of safety for the host system.

All terminals using bash are configured to start with the appropriate virtual
environment activated.

Running the Pipeline
********************

1. Open a Terminal

2. Go to ``/home/goodman/data``

      ``cd /home/goodman/data``

3. Here you have a workspace to put your data according to your institution.

   .. image:: img/screenshot_1.png
       :width: 1200px

4. Create a data folder inside your workspace.

      ``cd NOAO``

      ``mkdir 2017-07-05``

      ``cd 2017-07-05``

5. Copy your data from Goodman Computer

       ``scp observer@soaric7:/home3/observer/GOODMAN_DATA/NOAO/2017-07-05/ ./``

6. Make sure you have a full data set. At this point your observing logs will
   become very useful, eliminate focus sequence, aquisition exposure and any
   other file present that will not be needed for the processing. The following
   list summarizes the kind of data that you need to fully process your data.

    - BIAS: Bias
    - FLAT: Flats
    - COMP: Comparison Lamps
    - OBJECT: Science Frames

   Also make sure your data has the same *readout speed*, *binning*, and *ROI*.
   If you used different configurations during the same night, we recommend you
   to set up a separate folder for each.

7. Run ``redccd``:

   If you are running ``redccd`` for the first time you can use ``redccd`` alone
   but if it's a second or third time you will need to use ``--auto-clean``
   which is a built-in protection for your data, in case you don't want to
   delete what has been done. Also you might want to consider
   ``--saturation <new value>`` to change the saturation level if you get all
   your flats rejected due to saturation. Sometimes there is a hot column at the
   end that produced very high values.

       ``redccd --auto-clean``

   In case you want to use ``--saturation`` here is an example:

       ``redccd --auto-clean --saturation 70000``

   This changes the saturation level to `70000 ADU`` in this context
   the saturation value works as a threshold for rejecting images and it varies
   from one instrument configuration to another.


   By default, ``redccd`` puts reduced data in a subdirectory ``RED``, you can
   provide a different one by using ``--red-path``.
   
   An image ``image_file.fits`` that has been fully (and properly) processed should
   have the new name (including the reduced data folder):
   
       ``cfzsto_image_file.fits``


8. Run ``redspec``:

   By default ``redspec`` will search for images with the prefix ``cfzsto``, in case
   you have produced a different prefix you can change it by using ``--search-pattern``

   You can just run ``redspec`` in case everything is the default but if this is
   the first time you run the pipeline we suggest:

       ``redspec --plot-results``

   In that way two important plots will be shown full screen, the comparison lamp
   fitted to a reference comparison lamp and some values for the wavelength solution
   fit and the extracted spectrum plotted with the wavelength solution.

   Before the wavelength solution is calculated, the extracted spectrum (1D already)
   is saved with an ``e`` as prefix. The final image has a ``w`` added to the
   start of the name, following the above example your final 1D and wavelength
   calibrated image will be named:

      ``wecfzsto_image_file.fits``

9. Finally, review the results. Below is a table with the definition of all
   letters used in the construction of the prefix.

     The meaning of every letter in ``wecfzsto`` is summarized in the following table:

   +--------+-----------------------------------------------------------------------+
   | Letter | Meaning                                                               |
   +========+=======================================================================+
   |    w   | Wavelength calibrated                                                 |
   +--------+-----------------------------------------------------------------------+
   |    e   | Extracted spectrum, 1D                                                |
   +--------+-----------------------------------------------------------------------+
   |    c   | Cosmic ray cleaned or mask created depending on the method.           |
   +--------+-----------------------------------------------------------------------+
   |    f   | Flat corrected                                                        |
   +--------+-----------------------------------------------------------------------+
   |    z   | Zero or Bias corrected                                                |
   +--------+-----------------------------------------------------------------------+
   |    s   | Slit trimmed, trims off the non-illuminated sections of the detector  |
   +--------+-----------------------------------------------------------------------+
   |    t   | Initial Image trimming                                                |
   +--------+-----------------------------------------------------------------------+
   |    o   | Overscan corrected                                                    |
   +--------+-----------------------------------------------------------------------+



.. include:: interactive_mode.rst

Troubleshooting
***************

- The wavelength Solutions is way off: Check that the lamp was correctly
  registered in the header. Also check that the corresponding reference lamp exist.
  for instance is not the same to have ``HgArNe`` to ``HgAr``
- Can't detect any objects: Check that the keyword ``OBSTYPE`` is correct.
- The reference data plot, in interactive mode, doesn't show anything or only
  vertical dotted lines: The reference lamp doesn't exist for that configuration,
  since this is used only for visual reference sometimes it will display the same
  lamp but in other instrument configuration, this will not affect the quality
  of the solution.
