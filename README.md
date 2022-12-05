# Acoustic classification scripts
Model library for classification of acoustic monitoring data at ZSL, including Colab notebooks for classifying data. Current models include:

- [BirdNet](https://github.com/kahst/BirdNET-Analyzer) - for classifying bird species (predominantly European and American species)
- [CityNet](https://github.com/mdfirman/CityNet) - for estimating the level of biotic and anthropogenic sound in urban landscapes
- [BatDetect](https://github.com/macaodha/batdetect) - classifying presence of bats in sound files, not to species level


# Image classification
- [Megadetector](https://github.com/microsoft/CameraTraps/blob/main/detection/megadetector_colab.ipynb) - classifying camera trap images

# Running notebooks in GCP using Vertex AI
Google Cloud's Vertex AI services will allow you to run Jupyter Notebooks within the cloud, utilising the cloud resources, and accessing the data stored in your GC bucket. 

**Ensure you stop the notebook when you are finished using it otherwise it will charge for the entire time it is not stopped, regardless of whether you are using it**

Instructions for running notebooks within Vertex AI:

## Opening an existing notebook ##
1. Navigate to Vertex AI workbench
2. There are existing workbooks for running citynet, birdnet, and bat detect.
3. 


## Starting a new notebook ##

1. Navigate to the Vertex AI workbench, and enable the API if not already done
2. From within the workbench, select 'User managed notebook' and create a new one. (Note: this is not the same as the Managed Notebook, and some services won't work - e.g. accessing Cloud Storage)
    * Follow instructions, set area to your closest location
    * Selection of compute resources depends on the script you will run:
        - BatDetect - select 1 GPU, and PyTorch environment
        - CityNet - select 1 GPU and Tensorflow 2.8 environment
        - BirdNet - No GPUs, 16 CPUs and Tensorflow 2.8 environment
- Within the notebook, you can mount the files stored in the Google Cloud Bucket using the following command (replacing 'bucket_name' with the name of your bucket):<br/>
 
   ``` !mountpoint -q /home/jupyter/gcs && echo "mounted" || gcsfuse --implicit-dirs --rename-dir-limit=100 --disable-http2 --max-conns-per-host=100 bucket_name                 "/home/jupyter/gcs" ```
   
   Once mounted, you can now access the bucket files as if they are in your local directory.

