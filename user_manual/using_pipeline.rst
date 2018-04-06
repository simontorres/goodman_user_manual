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
   |   400   |  M2  | GG455  | HgAr   |
   +---------+------+--------+--------+
   |   400   |  M2  | GG455  | HgArNe |
   +---------+------+--------+--------+
   | 600-old | Blue | None   | HgAr   |
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

It is possible to add new lamps very easily you just need a raw lamp that meets
the following specifications with respect to your science project:

   - Same instrument configuration or mode
   - Same Grating
   - Same order blocking filter if present
   - Same binning
   - Same lamp/combination that you use in your observations
   - Smallest slit possible. Equal is OK too.

Then you can use the interactive mode or other software (such as IRAF) to produce
a wavelength-calibrated 1D spectrum. Now you have to options, identify the
system folder where the lamps that come with the package are saved and simply
put it there or put it in another directory and use the argument ``--reference-files``


   ``redspec --reference-files /path/to/ref-lamp-location``

Or contact ``storres [at] ctio [dot] noao [dot] edu``  and we will make it
available as a package file.

On Goodman's Radial Velocity Precision
**************************************
Here we present a summary of the best *radial velocity* precision that can be
obtained with a given configuration. The equations used are listed below.

.. math::

    R = \frac{\lambda}{\Delta\lambda} = \frac{c}{v}

Then,

.. math::

    v = \frac{c}{R}

We can calculate the central wavelength for a given configuration which will
correspond to :math:`\lambda ` and :math:`\Delta\lambda` is the dispersion in
units of *Angstrom/Pixel* obtained from the Goodman Spectrograph Cheat Sheet.

The smallest grating available is the 0.45", then:

.. math::

    FWHM = \frac{slit-size}{pixel-scale} = \frac{0.45}{0.15} = 3.0

Then the limiting factor is not the spectrograph's dispersion but the *FWHM*,

.. math::

   \Delta\lambda = 3 * dispersion


.. table::

    +---------+------+--------------------+------------+---------------------+-----------------+
    | Grating | Mode | Central Wavelength | Dispersion | Resolving Power (R) |  RV Limit       |
    +=========+======+====================+============+=====================+=================+
    |   400   |  m1  |      505.281       | 1.00       | 1516                |  197.773 km / s |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   400   |  m2  |      700.154       | 1.00       | 2100                |  142.727 km / s |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   600   |  UV  |      442.270       | 0.65       | 2041                |  146.867 km / s |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   600   | Blue |      492.529       | 0.65       | 2273                |  131.881 km / s |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   600   | Mid  |      578.827       | 0.65       | 2672                |  112.218 km / s |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   600   | Red  |      777.885       | 0.65       | 3590                |  83.502 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   930   |  m1  |      384.521       | 0.42       | 2747                |  109.151 km / s |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   930   |  m2  |      469.125       | 0.42       | 3351                |  89.466 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   930   |  m3  |      554.787       | 0.42       | 3963                |  75.652 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   930   |  m4  |      639.418       | 0.42       | 4567                |  65.639 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   930   |  m5  |      724.936       | 0.42       | 5178                |  57.896 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |   930   |  m6  |      809.084       | 0.42       | 5779                |  51.875 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m0  |      374.297       | 0.31       | 3622                |  82.765 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m1  |      424.181       | 0.31       | 4105                |  73.031 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m2  |      492.678       | 0.31       | 4768                |  62.878 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m3  |      561.804       | 0.31       | 5437                |  55.141 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m4  |      629.735       | 0.31       | 6094                |  49.193 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m5  |      699.087       | 0.31       | 6765                |  44.313 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m6  |      767.000       | 0.31       | 7423                |  40.389 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+
    |  1200   |  m7  |      835.851       | 0.31       | 8089                |  37.062 km / s  |
    +---------+------+--------------------+------------+---------------------+-----------------+


.. include:: headers.rst

.. raw:: pdf

    PageBreak

.. _`Using Pipeline`:

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
   :align: center
   :widths: auto


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
the port is ``1`` and your password is ``password``.
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
   |    c   | Cosmic ray cleaned or mask created depending on the method            |
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
