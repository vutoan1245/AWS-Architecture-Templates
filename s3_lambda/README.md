# AWS CloudFormation Template for S3 Triggered Lambda Function

This AWS CloudFormation template provisions an S3 bucket that triggers a Lambda function when an object is created. It includes all necessary permissions and configurations to set up the S3 bucket and Lambda function securely.

## Template Overview

The template includes the following resources:

- **S3TriggerLambdaFunction**: A Lambda function that is triggered by events in the S3 bucket.
- **LambdaInvokePermission**: Permission for the S3 bucket to invoke the Lambda function.
- **LambdaIAMRole**: IAM role with permissions required by the Lambda function.
- **S3BucketNotification**: S3 bucket with notifications configured to trigger the Lambda function on object creation.

## Parameters

- **NotificationBucket**: The name of the S3 bucket that will trigger the Lambda function.

## Resources

### S3TriggerLambdaFunction

A Lambda function written in Python 3.9 that prints the event details when an object is created in the specified S3 bucket.

- **Handler**: `index.lambda_handler`
- **Runtime**: `python3.9`
- **Timeout**: `30` seconds
- **Role**: IAM role specified by `LambdaIAMRole`

### LambdaInvokePermission

Permission for the S3 bucket to invoke the Lambda function.

- **FunctionName**: ARN of the `S3TriggerLambdaFunction`
- **Action**: `lambda:InvokeFunction`
- **Principal**: `s3.amazonaws.com`
- **SourceAccount**: AWS account ID
- **SourceArn**: ARN of the S3 bucket

### LambdaIAMRole

IAM role with the necessary permissions for the Lambda function to create log groups, log streams, and put log events.

- **AssumeRolePolicyDocument**: Trust policy allowing Lambda to assume this role
- **Policies**:
  - **PolicyName**: `root`
  - **PolicyDocument**:
    - **Action**:
      - `logs:CreateLogGroup`
      - `logs:CreateLogStream`
      - `logs:PutLogEvents`
    - **Resource**: `arn:aws:logs:*:*:*`

### S3BucketNotification

An S3 bucket with server-side encryption and public access blocked, configured to trigger the Lambda function when an object is created.

- **BucketName**: Name specified by the `NotificationBucket` parameter
- **BucketEncryption**: Server-side encryption with AES-256
- **PublicAccessBlockConfiguration**:
  - `BlockPublicAcls`
  - `BlockPublicPolicy`
  - `IgnorePublicAcls`
  - `RestrictPublicBuckets`
- **NotificationConfiguration**:
  - **LambdaConfigurations**:
    - **Event**: `s3:ObjectCreated:Put`
    - **Function**: ARN of the `S3TriggerLambdaFunction`

## Deployment Instructions

1. **Prerequisites**: Ensure you have the AWS CLI and necessary permissions to create the resources defined in the template.
2. **Upload the template**: Save the template as `template.yaml`.
3. **Create the stack**: Run the following command in the AWS CLI, replacing `<your-stack-name>` and `<your-bucket-name>` with appropriate values:

   ```sh
   aws cloudformation create-stack --stack-name <your-stack-name> --template-body file://template.yaml --parameters ParameterKey=NotificationBucket,ParameterValue=<your-bucket-name>
   ```

4. **Verify resources**: Once the stack creation is complete, verify the resources in the AWS Management Console.

## Notes

- The template suppresses certain guard rules for simplicity. These rules should be considered and enabled in a production environment for better security and compliance.
- Modify the Lambda function code as needed to handle different S3 events or to perform specific tasks based on your requirements.

## License

This project is licensed under the MIT License.
