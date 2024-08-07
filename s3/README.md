# S3 Bucket CloudFormation Template

This CloudFormation template creates an S3 bucket with specified properties.

## Template Description

The template creates an S3 bucket with the following properties:

- Name provided as a parameter
- Private access control
- Versioning enabled
- Tags for identification and environment

## Parameters

- `BucketName`: The name of the S3 bucket (required).

## Outputs

- `BucketName`: The name of the created S3 bucket.
- `BucketArn`: The Amazon Resource Name (ARN) of the created S3 bucket.

## Usage

### Prerequisites

- AWS CLI installed and configured with appropriate permissions.
- Alternatively, you can use the AWS Management Console.

### Steps to Deploy

1. Save the CloudFormation template as `s3-bucket-template.yaml`.

2. Deploy the template using the AWS CLI or AWS Management Console.

#### Using AWS CLI

```sh
aws cloudformation create-stack \
  --stack-name my-s3-bucket-stack \
  --template-body file://s3-bucket-template.yaml \
  --parameters ParameterKey=BucketName,ParameterValue=my-unique-bucket-name
```
