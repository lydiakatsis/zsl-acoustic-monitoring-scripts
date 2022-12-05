## Outline ##

This script will load in each audio file one by one, and classify the anthopogenic sound levels, then output a csv for each file with the mean anthroppgenic sound level.
Then it will reload each audio file again, and perform the classifications for the biotic sound. Currently this script is quite slow, as separate loading of audio files takes 10-15 seconds each time.
<br/><br/>

Results will be saved to **gs://acoustic-processing-outputs/city-net/FOLDERNAME/././.csv**
<br/><br/>

The final line of the script will concatenate all the csv's into one, with a column for anthropogenic and biotic sound level, and a row for each file. This will be saved
in **gs://acoustic-processing-outputs/city-net/.csv**
