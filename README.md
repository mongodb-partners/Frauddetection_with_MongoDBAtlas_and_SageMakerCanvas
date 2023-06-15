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

- Setup the [S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) to which the MongoDB Atlas data needs to be exported.

### Setup Atlas Cluster
- [Set up](https://www.mongodb.com/basics/clusters/mongodb-cluster-setup#:~:text=about%20storage%20capacity.-,Creating,-a%20MongoDB%20Cluster) a MongoDB Atlas free cluster.
- Configure the database for [network security](https://www.mongodb.com/docs/atlas/security/add-ip-address-to-list/) and access.


### Setup Atlas Application Services

- Setup the [Data Federation](https://www.mongodb.com/docs/atlas/data-federation/deployment/deploy-s3/)  in Atlas and register the S3 bucket created.

- Setup the Atlas Application services to create the [trigger and functions](https://www.mongodb.com/docs/atlas/app-services/triggers/scheduled-triggers/). The triggers are to be scheduled to write the data to S3 at a period frequency based on the business need for Model Training.

Please refer the [link](https://github.com/mongodb-partners/Frauddetection_with_MongoDBAtlas_and_SageMakerCanvas/blob/main/code/function_to_write_to_S3) for a sample script to write to S3 bucket.



9. Set the endpoint name and run the experiment.
<img width="1728" alt="Screenshot 2022-12-20 at 2 41 47 PM" src="https://user-images.githubusercontent.com/114057324/211984132-449fe245-20e0-41e5-a54f-f07d267be806.png">

10. Post training, the best model will be auto-deployed to the enpoint name you provided in the earlier step.
<img width="1728" alt="Screenshot 2022-12-20 at 9 22 46 PM" src="https://user-images.githubusercontent.com/114057324/211984212-b42fa1bc-3faa-4e32-82c4-c80436dd3d3b.png">


#### AWS Lambda

- Create a [lambda](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-awscli.html) function for calling the model endpoint
- Modify the below function to fit your needs.

````
import boto3

def lambda_handler(event, context):
    # Create an SageMaker client
    sagemaker = boto3.client('sagemaker-runtime')

    # Set the endpoint name and the content type of the request
    endpoint_name = event.get('endpoint_name')
    content_type = '<content_type>'

    # Set the input data for the request
    input_data = event.get('input')

    # Make the inference request
    response = sagemaker.invoke_endpoint(
        EndpointName=endpoint_name,
        ContentType=content_type,
        Body=input_data
    )

    # Get the response from the endpoint
    result = response['Body'].read()

    # Return the result
    return {
        'result': result
    }

````

With the above end-point you can make call to your sagemaker end-point to get inferences on live data.

## Conclusion
This is a reference architecture for application-driven analytics. This architecture can be extended for any other use cases like predictive analytics, forecasting etc. by configuring the input data and sagemaker endpoint. Reach out to partners@mongodb.com for any queries.

