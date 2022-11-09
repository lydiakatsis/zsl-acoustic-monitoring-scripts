# Acoustic classification scripts
Model library for classification of acoustic monitoring data at ZSL, including Colab notebooks for classifying data. Current models include:

- [BirdNet](https://github.com/kahst/BirdNET-Analyzer) - for classifying bird species (predominantly European and American species)
- [CityNet](https://github.com/mdfirman/CityNet) - for estimating the level of biotic and anthropogenic sound in urban landscapes
- [BatDetect](https://github.com/macaodha/batdetect) - classifying presence of bats in sound files, not to species level


# Image classification
- [Megadetector](https://github.com/microsoft/CameraTraps/blob/main/detection/megadetector_colab.ipynb) - classifying camera trap images

# Running notebooks in GCP using Vertex AI
Google Cloud's Vertex AI services will allow you to run Jupyter Notebooks within the cloud, utilising the cloud resources, and accessing the data stored in your GC bucket. Instructions for running notebooks within Vertex AI:

- Navigate to the Vertex AI workbench, and enable the API if not already done
- From within the workbench, select 'User managed notebook' and create a new one. (Note: this is not the same as the Managed Notebook, and some services won't work as nicely, so reccomend to stick to this for now)
    - Follow instructions, set area to your closest location

