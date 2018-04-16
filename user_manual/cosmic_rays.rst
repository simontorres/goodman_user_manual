Cosmic Rays Removal
*******************

The default cosmic ray removal tool has not been written in Python (neither for
Python). However it has been successfully integrated into the Pipeline's workflow
using ``subprocess``. Despite the integration being successful, the fact that is
a ``C`` program, designed to be run *standalone* from a terminal, it comes with
several challenges in terms of flexibility. The addition of keywords has been
dealt with by creating a modified version of the software (see `Install DCR`_).
But one of the trickiest part, and probably even more important, is the *parameter
configuration*. It uses an external ASCII file and that file HAS TO be named
exactly ``dcr.par``, also that file has to be located on the folder where the
program is called from (usually the data location folder).

The default parameters work very good for the default configuration of the
Goodman HT Spectrograph (Spectroscopic 1x1 for instance), but if you have binning
2x2 you will need to tune the parameters.

Since you can't parse the parameters as arguments the only option is to change
the values on the ``dcr.par`` file.

If you use several configurations your best option is to store custom ``dcr.par``
files and use the argument ``--dcr-par-dir <dcr-par-location>`` to tell the
pipeline where to look for a ``dcr.par`` file, with the limitation
that is has to be called ``dcr.par``.

The pipeline will copy the file to the folder where the data is stored.