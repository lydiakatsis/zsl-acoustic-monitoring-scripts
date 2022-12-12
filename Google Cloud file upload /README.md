# Google cloud tool for uploading data to Google Bucket

We will use the gcloud SDK tools for transfering data to our Google Buckets - this tool is very efficient, and automatically splits up the files into smaller chunks for transfer. Additionally, if the transfer is stopped for any reason such as a cut in internet, or the computer is turned off, it can easily be resumed - it will simply look at the differences between the two directories and resume copying from where it left off.

# Contents
1. [Run from Terminal](#terminal-commands)
2. [Run from Colab](#colab-commands)


***These instructions all assume you are typing directly into your computer terminal, if you wish to execute them from within a python environment, e.g. a JupyterNotebook, then just insert '!' before each command.***
# Terminal commands
## Set up

Step 1:
Set up requires downloading the Google Cloud SDK from [here](https://cloud.google.com/sdk/docs/install) 

Step 2: 
To use the Cloud SDK it needs to be initialised by signing into the GCP account. First open your terminal and type:

```
gcloud init
```

* Depending on whether you have previously authorized access to Google Cloud, you might be prompted to log in and grant access in a web browser or to select an existing account.

* Choose a current Google Cloud project if prompted.

* Choose a default Compute Engine zone if prompted. (London is europewest-2)

## Copying files from a local drive to your Google Bucket

You can use the Google Cloud User interface to create a new Bucket for your project, or select an existing Bucket. The path to your bucket will be the following:

```
gs://{bucket-name}
```
E.g. the path for the Network rail project is: `gs://nr-acoustic-data`

If you want to copy all the files within several folders to your Bucket, and also preserve the folder structure, then you can run the following command from your terminal:


```
gcloud storage cp -r '/path/to/data/' gs://nr-acoustic-data/
```


If you want to include wildcards (e.g. '*' to denote multiple options), you can use the following. (Note: the ' ' around the path may or may not be needed depending on the terminal type you are using, trial both and see which works.)

```
gcloud storage cp -r '/path/to/data/*/*/' gs://nr-acoustic-data/
```


## Example

This is how I coped all the .WAV files only from my hardrive into the bucket. Note - the folder hierarchy dissappears with this command and all .WAV files are placed directly into the bucket: e.g. gs://bucket/123.WAV

```
gcloud storage cp /Volumes/T7_Touch/ZSL/Data/*.WAV   gs://nr-acoustic-data/
```

This is how I copied all the folder directories within my external hardrive into the bucket, the resulting structure within the bucket was then exactly the same as that from my hard drive:

```
gcloud storage cp -r '/Volumes/T7_Touch/ZSL/Data/*/*/' gs://nr-acoustic-data/
```

# Colab commands

```
from google.colab import auth
auth.authenticate_user()
project_id = 'effortless-lock-365114'  # Set this to your project ID
!gcloud config set project {project_id}
!gsutil ls # Check that buckets appear here 

# Copy entire folder and all directory structure
!gcloud storage cp -r '/Volumes/T7_Touch/ZSL/Data/*/*/' gs://nr-acoustic-data/

```

