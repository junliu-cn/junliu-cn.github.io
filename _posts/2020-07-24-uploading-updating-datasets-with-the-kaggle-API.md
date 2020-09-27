---
title: 'Uploading & Updating Datasets with the Kaggle API'
date: 2020-07-24
permalink: /posts/2020/07/uploading-updating-datasets-with-the-kaggle-API/
tags:
  - Kaggle API
---

[Uploading & Updating Datasets with the Kaggle API](https://codelabs.developers.google.com/codelabs/upload-update-data-kaggle-api/index.html?index=..%2F..index#1)

### 1. Installing the API
Ensure you have Python 3 and the package manager pip installed.

To install the API, run the fiollowing command in the shell:

- pip install kaggle --user

### 2. Get your Kaggle credentials
In order to use the API, you will also need to have an API key associated with your Kaggle account stored locally.

#### Make or log into your Kaggle account
First, make sure you have an active Kaggle account. Go to Kaggle and click on the blue "Sign In" in the top, right hand corner. If you already have an account, log in. If you do not have an account, you can create a new one either by linking your Google, Yahoo or Facebook account or by clicking on the text "Manually create a new account" and following the prompts.

#### Download your credentials
Once you have created your account and logged in, you can find your API key by going to https://www.kaggle.com/<your username>/account, where <your username> is replaced with your actual Kaggle username. Then scroll down and click on the "Create New API Token" button under the API section (shown below).

This will download a .json file containing your credentials. Move this into the file in the location ~/.kaggle/kaggle.json on Mac/Linux, or C:\Users\<Windows-username>\.kaggle\kaggle.json on Windows. (If you don't have the .kaggle folder on your computer, using the command line to navigate to the folder above it, and then create the folder using mkdir .kaggle.)

For your security, ensure that other users of your computer do not have read access to your credentials. On Unix-based systems you can do this with the following command:

- chmod 600 ~/.kaggle/kaggle.json

### 3. Get Your Data Ready for Upload
#### Initialize the Dataset Folder
Once you have the Kaggle API installed and your credentials saved, you need to initialize your dataset folder. This will create a .json file in the correct format for you to put your metadata in.

Mac/Linux:

- kaggle datasets init -p ~/Documents/barley_data/

Windows:

- kaggle datasets init -p C:\Users\<user name>\Documents\barley_data\

### Edit the Metadata File
A dataset is more than just the data files: we need to make sure that we provide useful metadata so that other people will know what's in our dataset and how to work with it. (This is especially important for public datasets.)

When we initialized our dataset folder, a file datapackage.json was automatically created for us. If we open it up look at it, we'll see something like this:
```
{  
   "licenses":[  
      {  
         "name":"CC0-1.0"
      }
   ],
   "id":"<user name>/INSERT_SLUG_HERE",
   "title":"INSERT_TITLE_HERE"
}
```
Let's update it! Since this data was collected and released by the United States Federal government, it's in the public domain. This means that CC0 is the appropriate license for this dataset, so we don't need to change that. (You can see a full list of supported licenses here.) However, we will want to update the slug and dataset title to something more informative, to something like this:
```
{  
   "licenses":[  
      {  
         "name":"CC0-1.0"
      }
   ],
   "id":"<user_name>/usda-barley",
   "title":"USDA Barley Data"
}
```
Note that you want to replace <user_name> with your actual Kaggle username! Make sure to save your changes.

### 4. Upload data
Now that you're all set up, uploading your data is pretty simple! You can do it in one line, like so:

Mac/Linux:

- kaggle datasets create -p ~/Documents/barley_data/

Windows:

- kaggle datasets create -p C:\Users\<user name>\Documents\barley_data\

Once your dataset is finished uploading you should see the message: Dataset is being created. Please check progress at [dataset URL].

### Optional: finish documenting your dataset and make it public
By default, your dataset will be published privately and only be visible and usable to you. You don't have to make your dataset public, but you can if you like. If you do make your dataset publicly available or you plan to share it later you should document your dataset to make it easier for people to use. To do that, you can go to the URL for your dataset and follow the steps in the Quality Checklist using the graphical interface:

- Provide a description
- Provide a subtitle
- Upload a banner image
- Add tags

In order to make your dataset public, you will need to go to the page for your dataset and then go to the settings tab. (The url will look like this: https://www.kaggle.com/<user name>/<dataset name>/settings.) Select "Sharing" from the menu on the left hand side, then click on "Make Public" in the "Privacy" field.

Making your dataset public does not change the license of your dataset, it will just make it publically viewable and findable on Kaggle.

### 5. Update data
Once you've added a dataset to Kaggle, you can use the API to update it. You can update datasets to:

Add new files to your dataset
Update the data in an existing file
In this case, we're going to add a .csv file that has helpful information about the columns in our other data file. Make sure you're in the folder where the rest of your data is saved, and then run the following command in the console to download this file.

- curl https://raw.githubusercontent.com/rctatman/codelab_data/master/file_info.csv -O

Now that you have an additional file, you can update your dataset by running the following line of code. Note that the message you include will be visible to anyone you add to your dataset as a collaborator or to everyone if you make your dataset public, so try to make it helpful.

Mac/Linux:
- kaggle datasets version -p ~/Documents/barley_data/ -m "added info file with additional metadata"

Windows:
- kaggle datasets version -p C:\Users\<user name>\Documents\barley_data\ -m "added info file with additional metadata"

And that's all there is to it! If you have a dataset that you would like to update regularly, you can set up a cron job to update it at whatever intervals make sense given your dataset and how frequently it updates.
