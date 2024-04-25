
# Visualize Data using Amazon Quicksight

**Sample Scenerio**: ABC Company, a retail business, struggled to gain insights from their scattered sales data. They needed a centralized and interactive platform to visualize key metrics and identify trends to make data-driven decisions.

**My Role**: As a AWS professional, I will Amazon QuickSight to create a comprehensive sales dashboard. I will connect QuickSight to various data sources (e.g., CRM, sales reports) and transform the data into a usable format.


For this project, I will be using a list of data containing Amazon's best selling items.
The data was obtained from a data source called [Bright Data](https://brightdata.com/). Note that this data has been scraped and cleaned up. The data can be downloaded from this [repo](https://github.com/laraadeboye/AWS_projects/tree/main/visualize-data-with-amazonQuicksight)



## Outline

Step 1. Download a dataset of 50,000 best-selling Amazon products

Step 2. Store dataset into Amazon S3 bucket


Step 3. Connect the S3 bucket with Quicksight and create visualizations

Step 4. Create Visualizations


## Step 1. Download a dataset of 50,000 best-selling Amazon products 
The data containing Amazon best selling items can be downloaded from [this repo](https://github.com/laraadeboye/AWS_projects/tree/main/visualize-data-with-amazonQuicksight) (This data was obtained from [Brightdata](https://brightdata.com/) through [tech with Lucy](https://github.com/techwithlucy/youtube/tree/main/2-s3-quicksight))

&nbsp;

## Step 2. Store dataset into Amazon S3 bucket  
1. On the AWS console we will create an S3 bucket with the following details:

**Bucket name**: best-seller-1234789 (you may use any name you wish. Bucket names must be globally unique)

Keep the rest of the default and click **Create Bucket**

&nbsp;

2. Upload the CSV files to the bucket

3. We will make changes  to the manifest.json file. We will replace the `BUCKET-NAME`  with the name of the bucket we created save the file. Then upload the manifest.json file.
Click on the uploaded manifest.json file and **copy S3 URL**. Save the copied text on a text editor in your device for later use.

&nbsp;

##  Step 3. Connect the S3 bucket with QuickSight and create visualizations

1. Navigate to QuickSight console by searching in the services section. If you have not used QuickSight before, you will be required to sign up. Sign up and start with a free trial (Plan to cancel the subscription before 30days to avoid surprise charges)
2. Follow the prompts to Create your QuickSight account.
3. Navigate to the **Allow access and auto discovery for these resources** section, Click **Select S3 buckets**
4. We will follow the prompts to connect our newly created bucket to QuickSight. 
5. Give it some time for the QuickSight account to be created.
6. Click **Go to Amazon QuickSight**. Next Click **Datasets**, then click **New Datasets**, then click **S3**

We will name it **Amazon-best-seller-data**
We will paste  the manifest.json s3 URL under the **Upload a manifest file**

Then click **connect**;
Then click **visualize**


7. In the next dialogue box, click the **Interactive sheet** option (Allow a few minutes for the download to complete)

&nbsp;

## Step 4. Create Visualizations

Once the upload is complete, we can create visualizations for the following headings:
* availability
* brand
* price etc.

We can sort based on :
* **Most popular brands to the least popular brands**
* **Make comparisons between brands**

* **Compare the prices of different best-selling products**

* **Identify which sellers have the most best-selling products**

See below the images of the visualizations I created based on the queries.

[IMAGES]

## Conclusion

We have been able to create visualizations from large datasets uploaded to an S3 bucket.
