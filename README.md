# AWS CloudFormation Templates

Welcome to my collection of AWS CloudFormation templates! This repository contains various templates to help you quickly set up and manage different AWS architectures. Each template is designed to be reusable and configurable, making it easy to deploy complex infrastructure with minimal effort.

## Getting Started

To use these templates, you'll need an AWS account and the AWS CLI installed on your local machine. You can find instructions on how to install the AWS CLI [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).

## Deployment

You can deploy this template using the AWS Management Console or the AWS CLI. To deploy using the AWS CLI, save the template as `template.yaml` and run the following command:

```sh
aws cloudformation create-stack --stack-name my-static-website --template-body file://template.yaml --capabilities CAPABILITY_NAMED_IAM
```

Replace `my-static-website` with your desired stack name. This will create the necessary AWS resources to host a static website using S3 and CloudFront.

## License

This project is licensed under the MIT License.
