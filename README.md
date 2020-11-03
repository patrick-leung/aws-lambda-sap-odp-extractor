# VNSG SAP on AWS Workshop 
## Start Building your AWS Data Lake

Michiel Bijlsma and Patrick Leung - SAP on AWS Specialist Solution Architect

This is a sample application for extracting data from SAP applications (SAP S/4HANA, SAP ECC and SAP BW) using Operational Data Provisioning (ODP). You can find more information on ODP [here](https://blogs.sap.com/2017/07/20/operational-data-provisioning-odp-faq/). Operational Data Provisioning can expose the full load and delta data using OData services. This application package contains a Lambda layer to connect with SAP and consume the OData services as a REST API. Extracted data is saved to S3 Bucket. A DynamoDB table is also created to store the metadata for extracts. The package also contains a sample Lambda function to demonstrate usage of the lamdba layer

### Requirements
### All participants:
* An Amazon (shopping) account (AWS Console account will be generated for this lab)

### Instructors:
* SAP application (ABAP stack) with SAP Netweaver 7.5 or above. If required, you can create an SAP ABAP developer edition using cloud formation template [here](https://github.com/aws-samples/aws-cloudformation-sap-abap-dev)
* OData services for ODP based extraction are already created. It is assumed to you know about ODP and how OData services can be created from them. [This](https://help.sap.com/viewer/ccc9cdbdc6cd4eceaf1e5485b1bf8f4b/7.5.9/en-US/11853413cf124dde91925284133c007d.html) SAP documentation link provides information on how to expose ODP as OData services. You can find more information on ODP [here](https://blogs.sap.com/2017/07/20/operational-data-provisioning-odp-faq/).

### Event Engine
1. Go to the website  https://dashboard.eventengine.run/login
2. You will need to login with their Amazon Retail account in order to access an event using an event hash code from your instructors
3. Create your team name
4. Click on AWS console and you will get a pop up, choice Mac/Linux or Windows, copy the export paste into the terminal. Leave this terminal up and running as we will be using this for the deployment.    

### Setup Cloud9

Follow the [instruction](https://docs.aws.amazon.com/cloud9/latest/user-guide/setup-express.html) to create a cloud9 enironment. 

### Deploy Lambda Function for SAP ODP Extraction to Amazon S3

1. Clone this repo to a folder of your choice

2. Navigate to the root folder of the cloned repo and then perform the preparation steps.
```bash
cd aws-lambda-sap-odp-extractor
npm install
```
3. Navigate to the lib folder
```bash
cd lib
```
4. Update the appConfig.json file in the lib folder to suit your needs. At a minimum, update your account ID, region details.

5. Navigate to project root folder
```bash
cd ..
```

6. Bootstrap your AWS account for CDK. Please check [here](https://docs.aws.amazon.com/cdk/latest/guide/tools.html) for more details on bootstraping for CDK. Bootstraping deploys a CDK toolkit stack to your account and creates a S3 bucket for storing various artifacts. You incur any charges for what the AWS CDK stores in the bucket. Because the AWS CDK does not remove any objects from the bucket, the bucket can accumulate objects as you use the AWS CDK. You can get rid of the bucket by deleting the CDKToolkit stack from your account.
```bash
cdk bootstrap aws://<YOUR ACCOUNT ID>/<YOUR AWS REGION>
```

7. Deploy the stack to your account. Make sure your (AWS) CLI environment is setup for account ID and region provided in the appConfig.json file. 
(This is automatically set-up in case Cloud 9 is used as the CLI environment in the same region, just execute the deploy command below?)
```bash
cdk deploy
```

### Configuration and Testing

1. Create a Folder in the S3 Bucket

2. Open test Lambda function and update the environment variables 
  * dataS3Folder (created in the previous step) 
  * odpServiceName is ZEPM_DS_SALESORDER_SRV_01 
  * odpEntitySetName is EntityOf0EPM_DS_SALESORDER 
  * sapHostName (provided by the instructor)
  * sapPort is 50000

3. Create a test event and execute a test in the Lambda function. This should extract the data from backend SAP application and load it to the S3 bucket.

### Cleanup

In order to delete all resources created by this CDK app, run the following command
```bash
cdk destroy
```

