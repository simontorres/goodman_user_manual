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