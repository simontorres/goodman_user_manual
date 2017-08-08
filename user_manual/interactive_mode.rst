Using the Interactive Mode
^^^^^^^^^^^^^^^^^^^^^^^^^^

If you need to make sure that your solution is the best possible you can use
the *Interactive Mode*. Some of the key features are:

- Manually match spectroscopic lines
- Zoom in to get a better reference
- Evaluate the quality of your solution
- Uses a visual reference for matching the lines
- Uses laboratory values for reference lines

Right now it uses matplotlib as the underlying tool to enable interaction, this
has some limitations but

Below you will see a sequence as well as a description of the procedure to use
the interactive mode.

1. The interactive screen has four subplots. In the *Upper Left* you have
   the raw comparison lamp, i.e. the comparison lamp extracted but without
   wavelength solution, the dashed red line represents the lines identified by
   the pipeline. In the *Lower Left* corner you have the reference lamp which is
   a previously calibrated lamp that is distributed with the package (also you
   can use your own). In this case the red lines are laboratory lines obtained
   from a NOAO site. The *Upper Right* plot is a static help, is intended to give
   you a quick and easy-to-reach help. Finally, the *Lower Right* plot is a
   dynamic one, in fact it serves three main purposes:

   - Display messages and warnings.
   - Shows you a zoomed line.
   - Displays the wavelength solution quality information.
   
   .. image:: img/interactive_001.png

2. The way you interact is by using your mouse and keyboard. The following table
   describes all the available functions with its keys and/or buttons. It also
   contains a summary of its function.

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
   of keys and functions. Since we are still in development stage, if you feel
   that the use of a key creates problems for you let us know.

   In the following image the mouse pointer was placed very close to the center
   of the line and then with a middle button click the line is marked with a
   red circle. The software will calculate a centroid (vertical dashed blue line)
   and then will try to find the closest reference value (vertical red line).
   Keep in mind that the reference data (red dotted lines) are not the *reference
   lamp* lines themselves but values obtained from literature.

   .. image:: img/interactive_002.png

   Then you have to find the corresponding line in the opposite plot, there no
   preferred plot to start as long as you are consistent with your marks.

3. In case you miss-identified a line, like in the image below, you can place
   the mouse pointer above the corresponding red circle and press **d**
   to delete it. It will search for the closest mark along the horizontal axis
   and remove it. If there is a counterpart in the opposite plot it will delete
   it too.


   .. image:: img/interactive_bad-id_002.png
     :width: 100%

4. Once you matched a good number of lines, the minimum required by the fitting
   routines are four, you can either press F2 or f to make a fit of the pixels
   and angstrom values collected. Now the *Bottom Right* plot will show the
   difference in angstrom for pixel values. It is important to note here that
   the points in that plot do not represent the points you marked but each line
   detected (red dashed line in the Raw Data plot, *Upper Left*.) It does a 2
   sigma clip once to reject outliers and then it uses the values to calculate
   the Root Mean Square Error. In this case is a bit high, but we will fix that
   below.

   .. image:: img/interactive_004.png

5. If you see that your current solution is decent you can press F3 or a and
   the pipeline will try to find more points automatically, you will see them in
   the next plot near the bottom of each plot, at the level of the background.
   This is not perfect and indeed it depends on the preliminary wavelength
   solution, it uses the detected lines in the raw data, applies the preliminary
   solution and tries to find a match in the reference line values, most of the
   cases it improves the solution but not always so keep that in mind. In this
   case the RMS error is reduced by almost a half which is good but if you look
   closely you can see the mismatches, also the *Bottom Right* plot will show
   you this.

   .. image:: img/interactive_006.png
     :width: 100%

6. Now you will see an example of when the match is apparently good but is not,
   Here you can zoom in to see the details, unfortunately the raw and reference
   data are **NOT** synchronized so you have to try a couple of times to get the
   correct size ratios as in the plot below. So, what you do in this case is:
   First delete the points using d and then mark them again.

   .. image:: img/interactive_007.png
     :width: 100%

7. I recommend you doing a good check to all the points and then fit the solution
   again and you will have something like this:

   .. image:: img/interactive_012.png
     :width: 100%

   You see that the *Bottom Right* plot shows that the differences have a sinusoidal
   shape which is also a signal that the solution can be improved. There are ways
   this could be improved even further but this will be left for a later version.

8. Finally, a few samples of the data extracted by the pipeline.

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



















