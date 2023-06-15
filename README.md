# Unmasking Deception: Harnessing the Power of MongoDB Atlas and Amazon SageMaker Canvas for Fraud Detection


## Introduction

In the digital age, the financial sector faces growing risks from cybercriminals who exploit vulnerabilities and manipulate systems for personal gain and institutional disturbance. High-profile hacks and fraudulent transactions have shaken the industry, leaving both financial institutions and customers concerned.As technology evolves, so do the techniques employed by these perpetrators, making the battle against fraud a perpetual challenge. 

Existing fraud detection systems often grapple with a critical limitation: relying on stale data. In a fast-paced and ever-evolving landscape, relying solely on historical information is akin to fighting fraud with one hand tied behind your back. Cybercriminals continuously adapt their tactics, making it imperative for financial institutions to stay one step ahead. The newest tactics the criminals deploy often can be seen in the data.  That's where the power of operational data comes into play. 

By harnessing real-time, fraud detection models can be trained on the most accurate and relevant data available. MongoDB Atlas, a highly scalable and flexible  developer data platform, coupled with SageMaker Canvas, an advanced machine learning tool, presents a groundbreaking opportunity to revolutionize fraud detection. By leveraging operational data, the synergy of the MongoDB Atlas and SageMaker Canvas holds the key to proactively identifying and combating fraudulent activities, enabling financial institutions to safeguard their systems and protect their customers in an increasingly treacherous digital landscape.



## MongoDB Atlas

[MongoDB Atlas](https://www.mongodb.com/atlas), the developer data platform is an integrated suite of data services centered around a cloud database designed to accelerate and simplify how you build with data. Build faster and smarter with a developer data platform that helps solve your data challenges. MongoDB Atlas's document-oriented architecture, designed for agility and scalability, proves to be a game-changer. Its ability to handle massive amounts of data in a flexible schema empowers financial institutions to effortlessly capture, store, and process high-volume transactional data in real-time. This means that every transaction, every interaction, and every piece of operational data can be seamlessly integrated into the fraud detection pipeline, ensuring that the models are continuously trained on the most current and relevant information available. With MongoDB Atlas, financial institutions gain an unrivaled advantage in their fight against fraud, unleashing the full potential of operational data to create a robust and proactive defense system.
 

## Amazon Data Wrangler
[Amazon SageMaker Canvas](https://aws.amazon.com/sagemaker/canvas/) revolutionizes the way business analysts leverage AI/ML solutions by offering a powerful no-code platform. Traditionally, implementing AI/ML models required specialized technical expertise, making it inaccessible for many business analysts. However, SageMaker Canvas eliminates this barrier by providing a visual point-and-click interface to generate accurate ML predictions for classification, regression, forecasting, natural language processing (NLP), and computer vision (CV). SageMaker Canvas empowers business analysts to unlock valuable insights, make data-driven decisions, and harness the power of AI without being hindered by technical complexities. It boosts collaboration between business analysts and data scientists by sharing, reviewing, and updating ML models across tools. It brings the realm of AI/ML within reach, allowing analysts to explore new frontiers and drive innovation within their organizations.

## Architecture Diagram

<img width="1286" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/16eb8ddf-65be-4909-81c3-bd7fcd7c5150">






## Step-by-Step Instruction

### Setup S3 Bucket

Setup the [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) to which the MongoDB Atlas data needs to be exported.

### Setup Atlas Cluster

[Set up](https://www.mongodb.com/basics/clusters/mongodb-cluster-setup#:~:text=about%20storage%20capacity.-,Creating,-a%20MongoDB%20Cluster) a MongoDB Atlas free cluster.

Configure the database for [network security](https://www.mongodb.com/docs/atlas/security/add-ip-address-to-list/) and access.

Sample Test Data Reference : You can refer to [Kaggle](https://www.kaggle.com/competitions/ieee-fraud-detection/data) for sample data set subjected to license agreements.


### Setup Atlas Application Services

Setup the [Data Federation](https://www.mongodb.com/docs/atlas/data-federation/deployment/deploy-s3/)  in Atlas and register the S3 bucket created.

Setup the Atlas Application services to create the [trigger and functions](https://www.mongodb.com/docs/atlas/app-services/triggers/scheduled-triggers/). The triggers are to be scheduled to write the data to S3 at a period frequency based on the business need for Model Training.

Please refer the [link](https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/blob/main/code/function_to_write_to_S3) for a sample script to write to S3 bucket.

### Setup the AWS SageMaker

#### AWS SageMaker Domain 
[Setup](https://docs.aws.amazon.com/sagemaker/latest/dg/onboard-quick-start.html) the AWS SageMaker domain 

#### Open the Canvas Studio

<img width="626" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/ffc6b09a-7d62-474a-b1fd-60b861055ddc">

#### Data Load from S3
Load the data from S3 bucket to Amazon SageMaker Canvas.

<img width="626" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/4ae799db-8b7b-411b-9ba3-7f9395b7f8d7">

#### Merging Data
Merge the data from two sources – in this example:  Fraud_detection_transactions and Fraud_detection_identity.

<img width="630" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/1127b717-c215-42ce-9b2b-a81a20b6d46d">

##### Create Model
On successful creation of the join data set, click on the create model and provide an appropriate name for the model. Select the appropriate problem type based on what needs to be predicted.

<img width="622" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/14aa0142-5288-415a-90ff-c2b74aad1947">

#### Data Visualization
The data visualization link provides rich visualization to deep dive on the data quality. It supports scatter diagrams, bar charts, line diagrams and more.

<img width="629" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/201173c1-ee4d-403d-b0fc-158597ed1786">


#### Data Validation
The data validation option helps to evaluate the data quality. It identifies the columns with missing data and highly skewed ones.

<img width="629" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/934001ee-a22d-40ab-95f3-2bf20ea8eccb">


#### Model Selection
In the model selection phase, various machine learning algorithms and techniques can be explored and evaluated to determine the most suitable approach for fraud detection. There are options for Standard Build or Quick Build, i.e. the choice between accuracy and speed. 
Selection of Target Column
Select the target column.  This is the column you are predicting.

<img width="634" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/2b2a2a59-f047-475c-981f-7918ce026983">

#### Build 
The build tab provides the following two options to build: 
Standard build:  This option is a production equivalent build which requires a minimum of 50K records. It is time consuming and highly accurate. 
Quick Start: This option is generally used where we are looking for a quick turnaround on the inference, but with less accuracy. 

<img width="634" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/aa162a72-a9d6-4b08-900b-6a8a8796091d">



#### Deploy and Analyze
Once the model is trained and optimized, it can be deployed and integrated into your fraud detection system.

<img width="632" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/a9c3f4aa-69e0-4693-ab7d-c0e77a04162b">


#### Inference
Your model can be used for both batch inference, where large volumes of data can be processed offline, or real-time inference, enabling the system to identify and flag fraudulent activities in real-time.

<img width="632" alt="image" src="https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/assets/101570105/b8862f3e-c1a2-47c7-9aa6-1f8ddcc0ce70">

## Application Driven Analytics
By integrating application driven analytics into this approach, we can enhance the fraud detection process further. Application driven analytics focuses on embedding analytical insights directly into business applications, enabling real-time decision-making and proactive fraud prevention. With this integration, the fraud detection model generated using Amazon SageMaker Canvas and powered by MongoDB Atlas and S3 can provide seamless and actionable insights within the banking applications used by customers and employees. By presenting fraud risk scores, alerts, and recommendations directly within the application interface, financial institutions can empower their users to make informed decisions, quickly respond to potential fraudulent activities, and prevent fraudulent transactions before they occur. 

## Conclusion
The battle against fraud in the financial sector requires innovation and agility. MongoDB Atlas and Amazon SageMaker Canvas stand as formidable allies in this ongoing fight, offering a transformative approach to fraud detection. By harnessing operational data and leveraging the power of real-time insights, financial institutions can fortify their defenses against cybercriminals who seek to exploit vulnerabilities for illicit gains. MongoDB Atlas proves its strength as an operational data store, accommodating high-volume transactional data with exceptional performance and flexibility. Meanwhile, Amazon SageMaker Canvas empowers business analysts to leverage AI/ML solutions effortlessly, providing a no-code platform that brings the power of advanced analytics to their fingertips. 

