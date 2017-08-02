Overview
########

The Goodman Spectroscopic Data Reduction Pipeline - GOODSPEC - is a Python-based
package for producing science-ready, wavelength-calibrated, 1-D spectra. The
goal of **goodspec** is to provide SOAR users with an easy to use, very well
documented software for reducing spectra obtained with the Goodman spectrograph.
Though the current implementation assumes offline data reduction, our aim is to
provide the capability to run it in real time, so 1-D wavelength calibrated
spectra can be produced shortly after the shutter closes.

The pipeline is primarily intended to be run on a data reduction dedicated
computer. Instructions for running the software are provided in the
`Using Pipeline`_ section of this guide.
The Goodman Spectroscopic Data Reduction Pipeline project is hosted at GitHub at
`this link <https://github.com/soar-telescope/goodman>`_.
Currently the pipeline is separated into two main components. The initial
processing is done by ``redccd``, which trims the images, and carries out bias
and flat corrections. The spectroscopic processing is done by ``redspec`` and
carries out the following steps:

- Identifies multiple targets (spectra of more than one object in the slit)
- Trace the spectra
- Extract the spectra
- Do background subtraction
- Find the wavelength solution. Defaults to automatic wavelength solution, but
  can be done interactively
- Linearize data (resample)
- Write wavelength solution to FITS header
- Create a new file for the wavelength calibrated 1D spectrum

Features
########
- Self-contained, full data reduction package for the most commonly used
  spectroscopic setups with Goodman.  Given the almost limitless number of
  possible configurations avaliable with the Goodman instrument, only the most
  popular configurations will be supported, though we will try to add as many
  modes as possible.
- Python based, using existing Astropy libraries as much as feasible.
- Extensively documented, using general coding standards: PEP8 – Style Guide,
  PEP257 – Docstrings Convention (in-code documentation) – Google Style
- Multiplataform compatibility (tested on Linux Ubuntu, CentOS and MacOSX).
- Modular design. Could be used as a library within other Python applications.


Ways to run the pipeline
########################
There are two ways to use the pipeline.

1. **Run it directly on a SOAR data reduction server** that you can access
   using VNC.

2. **Download and install the pipeline** (go to the Install_ section of this
   manual). Though we will try our best to provide answers to quick and simple
   installation issues, we cannot provide general installation support.

What the pipeline does not do
#############################
- In its current version the pipeline does not perform combination of individual
  spectra. If you obtained several individual exposures of the same object, they
  will be output as separate 1-D, wavelength-calibrated spectra

- There is yet no flux calibration. We are working on a module that will do this.

- The pipeline does not do miracles. If your have saturated flats, or your flats
  were taken with a different slit than your science frames, or you have no bias
  frames, the pipeline will fail.

