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


The training images are publicly available in a Cloud Storage bucket. Use the gsutil command line utility for Cloud Storage to copy the training images into your bucket:

` gsutil -m cp -r gs://automl-codelab-clouds/* gs://${BUCKET} `


## Create a dataset

Now that your training data is in Cloud Storage, you need a way for AutoML Vision to access it. You'll create a CSV file where each row contains a URL to a training image and the associated label for that image.

Run the following command to copy the file to your Cloud Shell instance:

` gsutil cp gs://automl-codelab-metadata/data.csv . `

Then update the CSV with the files in your project:

` sed -i -e "s/placeholder/${BUCKET}/g" ./data.csv `

Navigate back to the AutoML Vision Datasets page.

At the top of the console, click + NEW DATASET.

Type clouds for the Dataset name.

Leave Single-label Classification checked.


Click CREATE DATASET to continue

![Image of 1](https://github.com/IamVigneshC/GCP-AutoMLVision-ClassifyImagesofClouds/blob/master/1.png)


choose the location of your training images (the ones you uploaded in the previous step)

Choose Select a CSV file on Cloud Storage and add the file name to the URL for the file you just uploaded - gs://your-project-name-vcm/data.csv. You may also use the browse function to find the csv file. Once you see the white in green checkbox you may select CONTINUE to proceed.

After you are returned to the IMPORT tab, navigate to the IMAGES tab. It will take 8 to 12 minutes while the image metadata is processed. Once complete, the images will appear by category.

## Inspect images


![Image of 2](https://github.com/IamVigneshC/GCP-AutoMLVision-ClassifyImagesofClouds/blob/master/2.png)

To see a summary of how many images you have for each label, click on LABEL STATS. You should see the following pop-out box show up on the right side of your browser. Press DONE after reviewing the list.

![Image of 3](https://github.com/IamVigneshC/GCP-AutoMLVision-ClassifyImagesofClouds/blob/master/3.png)


## Train your model

Start training your model! AutoML Vision handles this for you automatically, without requiring you to write any of the model code.

To train your clouds model, go to the TRAIN tab and click START TRAINING.

Enter a name for your model, or use the default auto-generated name.

Leave Cloud hosted selected and click CONTINUE.

For the next step, type the value "8" into the Set your budget box and check "Deploy model to 1 node after training." This process (auto-deploy) will make your model immediately available for predictions after testing is complete.

Click START TRAINING.

The total training time includes node training time as well as infrastructure set up and tear down.


## Evaluate your model

After training is complete, click on the EVALUATE tab. Here you'll see information about Precision and Recall of the model. It should resemble the following:


![Image of 4](https://github.com/IamVigneshC/GCP-AutoMLVision-ClassifyImagesofClouds/blob/master/4.png)

You can also adjust the Confidence threshold slider to see its impact.

Finally, scroll down to take a look at the Confusion matrix.

![Image of 5](https://github.com/IamVigneshC/GCP-AutoMLVision-ClassifyImagesofClouds/blob/master/5.png)

This tab provides some common machine learning metrics to evaluate your model accuracy and see where you can improve your training data. Since the focus  was not on accuracy, move on to the next section about predictions section.


## Generate predictions


There are a few ways to generate predictions. Here you'll use the UI to upload images. You'll see how your model does classifying these two images (the first is a cirrus cloud, the second is a cumulonimbus).

First, download these images to your local machine by right-clicking on each of them (Note: You may want to assign a simple name like 'Image1' and 'Image2' to assist with uploading later):

Navigate to the TEST & USE tab in the AutoML UI:

On this page you will see that the model you just trained and deployed is listed in the "Model" pick list.

Click UPLOAD IMAGES and upload the cloud sample images you just saved to your local disk (you may select both images at the same time).

When the prediction request completes you should see something like the following:

![Image of 6](https://github.com/IamVigneshC/GCP-AutoMLVision-ClassifyImagesofClouds/blob/master/6.png)


The model classified each type of cloud correctly!
