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

![Screenshot 2022-12-05 at 10 35 38](https://user-images.githubusercontent.com/72734966/205617615-db47b28c-90c7-4326-9bc2-934babf92580.png)


Instructions for running notebooks within Vertex AI:

## Opening an existing notebook ##
1. Navigate to Vertex AI workbench
2. There are existing workbooks for running citynet, birdnet, and bat detect as shown above.
3. Select 'Open JupyterLab' next to the notebook to open it and use it (this will take a minute or two to start the instance)
4. When you are finished using it, stop the instance by selecting the box next to the notebook, and then pressing stop.
5. If the notebooks above aren't there, then start a new notebook following instructions below.


## Starting a new notebook ##

1. Navigate to the Vertex AI workbench, and enable the API if not already done
2. From within the workbench, select 'User managed notebook' and create a new one. (Note: this is not the same as the Managed Notebook, and some services won't work - e.g. accessing Cloud Storage)
    * Follow instructions, set area to your closest location
    * Selection of compute resources depends on the script you will run:
        - BatDetect - select 1 GPU, and PyTorch environment
        - CityNet - select 1 GPU and Tensorflow 2.8 environment
        - BirdNet - No GPUs, 16 CPUs and Tensorflow 2.8 environment
3. Open JupyterLab
4. Download the .ipynb script that you want to use from this repository , and drag it into the finder on the JupyterLab screen
5. Open the notebook, and follow the instructions within.

# Handy Vertex AI Notebook commands #

- Within the notebook, you can mount the files stored in the Google Cloud Bucket using the following command (replacing 'bucket_name' with the name of your bucket):<br/>
 
   ``` !mountpoint -q /home/jupyter/gcs && echo "mounted" || gcsfuse --implicit-dirs --rename-dir-limit=100 --disable-http2 --max-conns-per-host=100 bucket_name                 "/home/jupyter/gcs" ```
   
   Once mounted, you can now access the bucket files as if they are in your local directory.
   
   
   
- You can copy anything from your Google Cloud bucket into your notebook directory, or vice versa using Gcloud storage commands, e.g.: <br/>

   ``` !gcloud storage cp -r 'gs://data-processing-scripts/batdetect_v3-master/' . ```

