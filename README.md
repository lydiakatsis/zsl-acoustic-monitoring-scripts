Repository for classification algorithms and scripts used for ZSL Monitoring and Technology Prgoram ARU deployments.  

# Contents
   1.  [Data upload tips](#data-upload)
   2.  [Classification algorithms](#acoustic-classification-scripts) 
   3.  [Validation scripts](#validation)
   4.  [Options to run the scripts](#script-options)
         * [Vertex AI](#running-notebooks-in-gcp-using-vertex-ai)
         * [Google Colab](#google-colab)

# Data upload
## File hierarchy
* Store raw data and outputs in 2 separate locations

**Raw data:**
* Raw data upload follows the hierarchy and folder naming of:
   * {raw data bucket} = nr-acoustic-data
      * Subfolder = project+year e.g. [nr-2021]
         * Metadata
            * csv with lat long, and deployment details
         * Config e.g. [bat-config]
            * SD card name [e.g. MSD-10]
               * .WAV files 

**Outputs:**
* {Outputs bucket} = processing-outputs
   * Model folder [e.g. birdnet]
      * Project+year [e.g. nr-2021]
         * SD card name
            * Csvs of results

## GCP buckets
For uploading data from SD cards / Hard drive to Google Cloud bucket, use the GCloud storage tool, following instructions [here](https://github.com/lydiakatsis/zsl-acoustic-monitoring-scripts/tree/main/Google%20Cloud%20file%20upload%20)

# Acoustic classification scripts
## Outline
Collection of machine learning classifiers that will process the raw acoustic data and perform classifications. Results will be output to csv files in a separate output directory. Subsequent validation of results is essential.

## Model library
Model library for classification of acoustic monitoring data at ZSL, including Colab notebooks for classifying data. Current models include:

- [BirdNet](https://github.com/kahst/BirdNET-Analyzer) - for classifying bird species (predominantly European and American species)
- [CityNet](https://github.com/mdfirman/CityNet) - for estimating the level of biotic and anthropogenic sound in urban landscapes
- [BatDetect](https://github.com/macaodha/batdetect) - classifying presence of bats in sound files, not to species level

# Validation
Manual validation of machine learning results is essential. We have scripts for 2 general approaches for validation:

1. Check a random sample of 'positive' results for each species
   * This gives general level of uncertainty
   * May then move forward with machine learning outputs with high level of certainty for anaylses such as relative detection rates utilising all unverified results above chosen threshold  
   * Script selects random 50 sound samples classified as each species, and presents spectrogram and sound for checking. Verifications are input into a csv.

2. Top-down listening at each AudioMoth site
   * This confirms presence of each species at each site for spatial distribution of occurrence
   * For occupancy modeling would need to do this at intervals
   * Scripts selects all sites with species x classification above a chosen threshold, and then orders classifications for each site from highest score to lowest. Listen to the files until confirm presence for that site, then move on to the next site. Results output to a csv.

# Script options
These scripts are all written in Python and can be run from the command line as python files, from Google Colab, or from Vertex AI in GCP.

# Running notebooks in GCP using Vertex AI
Google Cloud's Vertex AI services will allow you to run Jupyter Notebooks within the cloud, utilising the cloud resources, and accessing the data stored in your GC bucket. 

**Ensure you stop the notebook when you are finished using it otherwise it will charge for the entire time it is not stopped, regardless of whether you are using it**

![Screenshot 2022-12-05 at 10 35 38](https://user-images.githubusercontent.com/72734966/205617615-db47b28c-90c7-4326-9bc2-934babf92580.png)


Instructions for running notebooks within Vertex AI:

## Opening an existing notebook ##
1. Navigate to Vertex AI workbench
2. There are existing workbooks for running citynet, birdnet, and bat detect as shown above.
3. Select 'Open JupyterLab' next to the notebook to open it and use it (this will take a minute or two to start the instance)
4. When you are finished using it, stop the instance by selecting the box next to the notebook, and then pressing stop.
5. If the notebooks above aren't there, then start a new notebook following instructions below.


## Starting a new notebook ##

1. Open Google Cloud console in web browser: https://console.cloud.google.com/ 
2. Navigate to the Vertex AI workbench, and enable the API if not already done
3. From within the workbench, select 'User managed notebook' and create a new one. (Note: this is not the same as the Managed Notebook, and some services won't work - e.g. accessing Cloud Storage)
    * Follow instructions, set area to your closest location
    * Selection of compute resources depends on the script you will run - below options have good performance for these scripts:
        - BatDetect - select 1 GPU, and PyTorch environment
        - CityNet - select 1 GPU and Tensorflow 2.8 environment
        - BirdNet - No GPUs, 16 CPUs and Tensorflow 2.8 environment
4. Open JupyterLab
5. Download the .ipynb script that you want to use from this repository , and drag it into the finder on the JupyterLab screen
6. Open the notebook, and follow the instructions within.

## Handy Vertex AI Notebook commands ##

- Within the notebook, you can mount the files stored in the Google Cloud Bucket using the following command (replacing 'bucket_name' with the name of your bucket):<br/>
 
   ``` !mountpoint -q /home/jupyter/gcs && echo "mounted" || gcsfuse --implicit-dirs --rename-dir-limit=100 --disable-http2 --max-conns-per-host=100 bucket_name                 "/home/jupyter/gcs" ```
   
   Once mounted, you can now access the bucket files as if they are in your local directory.
   
   
   
- You can copy anything from your Google Cloud bucket into your notebook directory, or vice versa using Gcloud storage commands, e.g.: <br/>

   ``` !gcloud storage cp -r 'gs://data-processing-scripts/batdetect_v3-master/' . ```

# Google colab
Scripts can be opened and run from Colab by clicking on the Colab scripts within each model folder.

## Handy Google Colab commands #

- Within the Colab notebook, you can mount the files stored in the Google Cloud Bucket using the following command (replacing 'bucket_name' with the name of your bucket, and project_ID with your project ID):<br/>

```!echo "deb http://packages.cloud.google.com/apt gcsfuse-bionic main" > /etc/apt/sources.list.d/gcsfuse.list
!curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
!apt -qq update
!apt -qq install gcsfuse

from google.colab import auth

# Authtication and PROJECT_ID allocation
auth.authenticate_user()
PROJECT_ID = "zsl-acoustic-pipeline"

!mountpoint -q /content/gcs_raw && echo "mounted" || mkdir -p gcs_raw; gcsfuse --implicit-dirs --rename-dir-limit=100 --disable-http2 --max-conns-per-host=100 "bucket_name" "/content/gcs_raw"
```
