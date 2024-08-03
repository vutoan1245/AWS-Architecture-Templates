# AWS CloudFormation Template for a Simple Website with VPC, ALB, and ASG

This AWS CloudFormation template deploys a simple website on EC2 instances within a VPC, an Application Load Balancer (ALB), and an Auto Scaling Group (ASG). The website displays the message "Welcome to my website: [instance IP address]".

## Template Overview

The CloudFormation template includes the following resources:

1. **VPC**: A Virtual Private Cloud to host the infrastructure.
2. **Subnets**: Two subnets in different Availability Zones for high availability.
3. **Internet Gateway**: To allow internet access to the VPC.
4. **Route Table**: To manage routing within the VPC.
5. **Security Group**: To allow HTTP access on port 80.
6. **Launch Configuration**: Defines the configuration for EC2 instances.
7. **Auto Scaling Group**: Ensures there are always 2 running instances.
8. **Application Load Balancer**: Distributes incoming traffic across the instances.
9. **Scaling Policy**: Automatically adjusts the number of instances based on CPU utilization.

## How to Use

### Prerequisites

- AWS CLI installed and configured with the necessary permissions.
- AWS CloudFormation service enabled in your AWS account.

### Steps

1. **Download the CloudFormation Template**

   Save the following template to a file named `template.yaml`

2. **Deploy the CloudFormation Stack**

   Use the AWS CLI to deploy the CloudFormation stack:

   ```bash
   aws cloudformation create-stack --stack-name my-website-stack --template-body file://template.yaml --capabilities CAPABILITY_NAMED_IAM
   ```

3. **Check the Outputs**

   Once the stack creation is complete, you can find the DNS name of the Load Balancer in the CloudFormation stack outputs. This DNS name can be used to access the website.

   ```bash
   aws cloudformation describe-stacks --stack-name my-website-stack --query "Stacks[0].Outputs[0].OutputValue" --output text
   ```

## Cleanup

To delete the resources created by this CloudFormation template, use the following command:

```bash
aws cloudformation delete-stack --stack-name my-website-stack
```

## Notes

- Ensure that your AWS account has the necessary permissions to create the resources defined in the template.
- The AMI ID (`ami-0c11a84584d4e09dd`) used in the template is specific to a particular region. You may need to update it based on the region you are deploying to.
- The security group allows inbound HTTP access from anywhere. Modify this as per your security requirements.

## License

This project is licensed under the MIT License.
