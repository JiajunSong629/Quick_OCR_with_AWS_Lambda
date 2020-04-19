# ECE 590.24 Data Analysis At Scale in Cloud

Previous projects:

[NEWS api on Google Cloud Platform](https://github.com/JiajunSong629/NEWS_API_on_Google_Cloud_Platform)

[Jupyter workflows using Docker Container](https://github.com/JiajunSong629/jupyter-workflows-using-docker-container)

[Spark jobs workflows on AWS EMR](https://github.com/JiajunSong629/AWS_EMR_Spark_Workflow)

## Serverless Data Engineering Pipeline

This is individual project 3 for my course, [Data Analysis At Scale in Cloud](https://noahgift.github.io/cloud-data-analysis-at-scale/).
In this project, I build an OCR application on AWS Lambda with Rekognition APIs to detect text in S3 Objects and stores labels in DynamoDB.
More features about the application are in the [screencast here](https://youtu.be/LbjLZYp2iME).

## Project structure

Here is a brief overview of the repo.

```bash
.
├── README.md                   <-- This instructions file
├── src                         <-- Source code for the Lambda function
│   ├── __init__.py
│   └── app.py                  <-- Lambda function code
├── template.yaml               <-- SAM template
└── SampleEvent.json            <-- Sample S3 event
```


### Requirements
* AWS CLI
* [Python 3.6 installed](https://www.python.org/downloads/)
* [Docker installed](https://www.docker.com/community-edition)

### How to build (on Cloud9 environment)

```bash
python3 -m venv ~/.ocrlambda  ## create virtual environment
pip3 install pip setuptools wheel pyyaml -U  ## update the pip version
pip install boto3 botocore awscli aws-sam-cli -U  ## install python SDK, aws and sam command line tools
sam init --location gh:aws-samples/cookiecutter-aws-sam-s3-rekognition-dynamodb-python  ## get the rekognition template
```

After a few configurations for example your the project structure is ready. You can check it by the following.

Next, `cd` into your directory, run the following command to create a S3 bucket, package the Lambda function and upload it to the bucket.

```bash
touch requirements.txt
sam build --use-container
aws s3 mb s3://your-bucket-name
sam package \
    --template-file template.yaml \
    --output-template-file packaged.yaml \
    --s3-bucket your-bucket-name
```

The sam deploy command will create a Cloudformation Stack and deploy the SAM resources.

```bash
sam deploy \
    --template-file packaged.yaml \
    --stack-name aws-sam-ocr \
    --capabilities CAPABILITY_IAM \
    --region your-region
```

## Resources:
- https://github.com/aws-samples/cookiecutter-aws-sam-s3-rekognition-dynamodb-python
- https://www.bilibili.com/video/BV1vJ411V7js