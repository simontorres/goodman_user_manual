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

.. include:: running_pipeline.rst

.. include:: troubleshooting.rst
