# GCP-AutoMLVision-ClassifyImagesofClouds


## Set up AutoML Vision

AutoML Vision provides an interface for all the steps in training an image classification model and generating predictions on it. Open the navigation menu and and select APIs & Services > Library. In the search bar type in "Cloud AutoML API". Click on the Cloud AutoML API result and then click ENABLE.

Create environment variables for your Project ID and Username:

` export PROJECT_ID=$DEVSHELL_PROJECT_ID `

` export USERNAME=<USERNAME> `
  
  
Create a Storage Bucket for the images you will use in testing.

` gsutil mb -p $PROJECT_ID \
    -c regional    \
    -l us-central1 \
    gs://$PROJECT_ID-vcm/  `
    
    
Open a new browser tab and navigate to the AutoML UI.

## Upload training images to Google Cloud Storage

In order to train a model to classify images of clouds, you need to provide labeled training data so the model can develop an understanding of the image features associated with different types of clouds. Our model will learn to classify three different types of clouds: cirrus, cumulus, and cumulonimbus. To use AutoML Vision we need to put the training images in Google Cloud Storage.

In the GCP console, open the Navigation menu and select Storage > Browser

Create an environment variable with the name of your bucket

` export BUCKET=YOUR_BUCKET_NAME `




