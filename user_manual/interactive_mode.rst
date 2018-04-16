Using the Interactive Mode
^^^^^^^^^^^^^^^^^^^^^^^^^^

.. warning::

    The automatic wavelength calibration method has become every time more
    reliable therefore is more likely that you will find bugs using the
    interactive mode.


If you are not convinced with the *automatic wavelength solution* you can try to
improve it by using the *Interactive Mode*. Some of the key features are:

- Manually match spectroscopic lines.
- Zoom in to get a better reference.
- Evaluate the quality of your solution as you progress.
- Uses a visual reference for matching the lines
- Uses laboratory values for reference lines

Right now it uses ``matplotlib.pyplot`` as the underlying tool to enable interaction.

Below you will see a sequence as well as a description of the procedure to use
the interactive mode.

1. The interactive screen has four subplots. In the *Upper main panel* you have
   the raw comparison lamp, i.e. your comparison lamp 1-D-extracted but without
   wavelength solution, the dashed red lines represent the lines found by
   the pipeline. In the *Lower main panel* you have the reference lamp. This is
   a previously calibrated lamp that is distributed with the package (also you
   can use your own). In this case the red lines are the laboratory values obtained
   from the NIST Atomic Spectra Database site (https://physics.nist.gov/PhysRefData/ASD/lines_form.html).
   The *Upper right panel* is a static help, intended to give
   you a quick and easy-to-reach help. Finally, the *Lower right panel* is an
   information window, in fact it serves three main purposes:

   - Display messages and warnings.
   - Shows you a zoomed line.
   - Displays the wavelength solution quality information.
   
   .. image:: img/interactive_001.png

2. The way you interact is by using your mouse and keyboard. The following table
   describes all the available keyboard commands with their respective keys and/or buttons,
   and a summary of their functionality.

   +----------+-----------------+------------------+----------------------------------------------------+
   | Main Key | Alternative Key | Mouse Equivalent | Description                                        |
   +==========+=================+==================+====================================================+
   |    ?     |       F1        |      None        |  Print Help message on the terminal                |
   +----------+-----------------+------------------+----------------------------------------------------+
   |    f     |       F2        |      None        |  Fit a function to collected points                |
   +----------+-----------------+------------------+----------------------------------------------------+
   |    a     |       F3        |      None        |  Find lines automatically                          |
   +----------+-----------------+------------------+----------------------------------------------------+
   |   None   |       F4        |      None        |  Evaluate wavelength solution                      |
   +----------+-----------------+------------------+----------------------------------------------------+
   |    d     |       F5        |      None        |  Remove point closest to the mouse pointer         |
   +----------+-----------------+------------------+----------------------------------------------------+
   |    l     |       F6        |      None        |  Linearize and smooth spectrum                     |
   +----------+-----------------+------------------+----------------------------------------------------+
   |    m     |      None       |  Middle Button   | Register the line closest to the mouse pointer     |
   +----------+-----------------+------------------+----------------------------------------------------+
   |  ctrl+z  |      None       |      None        |  Deletes ALL automatically added points            |
   +----------+-----------------+------------------+----------------------------------------------------+
   |  ctrl+d  |      None       |      None        |  Deletes all recorded points                       |
   +----------+-----------------+------------------+----------------------------------------------------+
   |  ctrl+q  |      None       |      None        | Ends the program                                   |
   +----------+-----------------+------------------+----------------------------------------------------+
   |  Enter   |      None       |      None        |  Accepts wavelength solution and closes the figure |
   +----------+-----------------+------------------+----------------------------------------------------+

   It is advisable to practice a little bit to get familiar with this combination
   of keys and functions. Since we are still in the development stage, if you feel
   that the use of a key creates problems for you let us know.

   In the following image the mouse pointer was placed very close to the center
   of the line and then with a middle button click the line is marked with a
   red circle. The software will calculate a centroid (vertical dashed blue line)
   and then will try to find the closest reference value (vertical red line).
   Keep in mind that the reference data (red dotted lines) are not the *reference
   lamp* lines themselves but values obtained from the NIST site indicated above.

   .. image:: img/interactive_002.png

   Then you have to find the corresponding line in the opposite plot. There is no
   preferred plot to start as long as you are consistent with the order in which you mark the lines.

3. In case you miss-identified a line, like in the image below, you can place
   the mouse pointer above the corresponding red circle and press **d**
   to delete it. It will search for the closest mark along the horizontal axis
   and remove it. If there is a counterpart in the opposite plot it will delete
   it too.


   .. image:: img/interactive_bad-id_002.png
     :width: 100%

4. Once you matched a good number of lines, the minimum required by the fitting
   routines are four, you can either press F2 or **f** to make a fit of the pixels
   and angstrom values collected. Now the *Lower Right panel* will show the
   scatter plot of the fit. It is important to note here that
   the points in this plot do not represent the points you marked but rather each of
   the lines detected in your extracted 1-D comparison lamp spectrum (red dashed lines
   in the *Upper main plot*) It does one iteration of a 2-sigma clippinp to reject outliers,
   and then it uses those values to calculate the Root Mean Square Error.
   In the example we show here the RMS is a bit high, but we will fix that below.

   .. image:: img/interactive_004.png

5. If you see that your current solution is decent you can press F3 or **a** and
   the pipeline will try to find more points automatically. The new matches found
   by the software are shown as red dots at the base of each line in the two main *Upper*
   and *Lower* panels.

   The automatic finding routine is not perfect, and indeed it depends on the preliminary wavelength
   solution. It uses the detected lines in the uncalibrated 1-D lamp, applies the preliminary
   solution and tries to find a match in the reference line values. In most
   cases it improves the solution, but not always so keep that in mind. In this
   case the RMS error is reduced by almost half, which is good, but if you look
   closely you can see the mismatches; also the *Bottom right* panel will show
   you this.

   .. image:: img/interactive_006.png
     :width: 100%

6. Now you we show an example of when the match is apparently good but in fact it's not.
   Here you need to zoom-in to see that there is an offset between the line centers and the
   matched laboratory line values, as shown in the figure below. You may have to apply different
   zoom values to your lamp and the reference lamp to get the plot to look like show it here.
   The solution in this case is locate the offending line(s), and delete it(them) pressing **d**.
   Then do a new match by clicking with the middle mouse button on the lines in the laboratory/reference
   plot, and the respective line in your 1-D uncalibrated lamp.

   .. image:: img/interactive_007.png
     :width: 100%

7. After you checked all the identifications and are happy with it, fit the solution
   again and you will obtain something like this:

   .. image:: img/interactive_012.png
     :width: 100%

   Here you can see that the *Bottom Right* panel shows the differences have a sinusoidal
   shape, which is also a sign that the solution can be improved. There are ways
   this can be implemented to refine the fit even further, but this is at present deferred to a later version.

8. Finally, a few samples of the spectra extracted by the pipeline.

   .. image:: img/interactive_013.png
     :width: 100%

   .. image:: img/interactive_014.png
     :width: 100%

   .. raw:: pdf

     Spacer 0 100

   .. image:: img/interactive_015.png
     :width: 100%

   .. image:: img/interactive_016.png
     :width: 100%

   .. raw:: pdf

     Spacer 0 100

   .. image:: img/interactive_017.png
     :width: 100%

   .. image:: img/interactive_018.png
     :width: 100%



















