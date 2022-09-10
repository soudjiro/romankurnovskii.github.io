---
title: Uploading a File to Amazon S3
description: A step-by-step guide to AWS Lambda 
toc: true
authors:
  - roman-kurnovskii
tags:
categories:
series:
date: "2022-05-23"
lastmod: "2022-05-21"
draft: false
weight: 3
---

## Uploading a File to Amazon S3

### Introduction

When you upload a folder from your local system or another machine, Amazon S3 uploads all the files and subfolders from the specified local folder to your bucket. It then assigns a key value that is a combination of the uploaded file name and the folder name. In this lab step, you will upload a file to your bucket. The process is similar to uploading a single file, multiple files, or a folder with files in it.

In order to complete this lab step, you have to upload the cloudacademy-logo.png file from your local file storage into an S3 folder you created earlier.

Download the Cloud Academy logo from the following location: https://s3-us-west-2.amazonaws.com/clouda-labs/scripts/s3/cloudacademy-logo.png (If the image is not downloaded for you, simply right-click the image and select **Save image as** to download it to your local file system.)

### Instructions

1. Click on the **cloudfolder** folder. You are placed within the empty folder in your S3 bucket:

![alt](./img/image-20220228120422-7-3f010e77-603f-4a07-b3b8-6029fd8c5a2a.png)

_Note_: Click the folder name itself, not the checkbox for the folder name. If you select the folder checkbox then upload a file, it will be placed above the folder (not inside it).

2. Click the **Upload** button.

3. Click **Add Files:**

![alt](./img/image-20220228120516-8-b4ad2d28-1118-4d61-9d86-4c8dae373d21.png)

A file picker will appear.

4. Browse to and select the local copy of _cloudacademy-logo.png_ file that you downloaded earlier:

![alt](./img/blobid0-138fb61c-9ce7-45dc-929d-47e3c6f1e7a2.png)

The logo is added to the list of files that are ready to upload. You have several options at this point:

* **Add more files**
* **Upload**

However, there is another method that some users prefer to add files for upload.

4. Check the file and click on **Remove:**

![alt](./img/image-20220228120654-10-c5d810b7-72d0-4a19-890c-0f8588aa2a08.png)

5. This time, rather than browsing to a file, drag and drop the logo file onto the wizard. The wizard adds it to the list of files to upload.

6. Scroll to the bottom of the page and click **Upload** to upload the file:

![alt](./img/image-20220228120632-9-68fa764f-29a9-4bc3-945d-cd52517e9876.png)

You will see a blue notification that the file is uploading and then a green notification that the upload has been completed successfully.

The file is placed in the folder in your bucket:

![alt](./img/blobid0-82d9ad93-78f2-46b1-bb68-383c056050ed.png)

### Summary

In this lab step, you learned two ways to upload files to Amazon S3.