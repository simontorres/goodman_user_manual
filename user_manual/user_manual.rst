Introduction
############

This document is the User Manual for the *Goodman Spectroscopic Data Reduction
Pipeline*. It provides an overview of the main features of the pipeline,
instructions on its use and how to run it on our dedicated *SOAR Data Reduction
Server*, and installation instructions for those who wish to run it on their own
computers.

.. include:: overview.rst

.. include:: using_pipeline.rst

.. include:: install.rst



Importing Parts of the Code
###########################

You can import almost every function in this pipeline:

.. code-block:: python
    :linenos:

    from goodman_ccd.core import spectroscopic_extraction
    # do some stuff here
    print('Hello World')




