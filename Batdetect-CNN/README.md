## Outline ##

This script runs the batdetect classification algorithm developed by Mac Aodha et al., 2018, which classifies presence or absence of bats. It then implements a newly developed element of this algorithm (unpublished) that classifies to species level. *Note this script uses a private GitHub repo, so will not work outside GCP* 

The script loads each sound file one by one, and performs a classification, then outputs a csv for that sound file in the results directory.

If the notebook periodically stops and becomes unresponsive, you can restart it and it will resume classifications from where it left off.

The final line in the script concatenates all the output csv's into one main one, with a row for each filename, and various results columns.
