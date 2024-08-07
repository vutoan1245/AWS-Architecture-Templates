Sure! Here is the content of the `readme.md` file:

# Simple Website Deployment on EC2 with SSH and CloudWatch

This AWS CloudFormation template deploys a simple website on an EC2 instance, configured with SSH access and monitored by CloudWatch.

## Template Details

- **AWSTemplateFormatVersion:** 2010-09-09
- **Description:** AWS CloudFormation Template to deploy a simple website on EC2 with SSH and CloudWatch.

## Resources

### Security Group

**Logical ID:** `MySecurityGroup`

- **Type:** `AWS::EC2::SecurityGroup`
- **Properties:**
  - **GroupDescription:** Enable HTTP and SSH access
  - **SecurityGroupIngress:**
    - Allow HTTP access (port 80) from anywhere
    - Allow SSH access (port 22) from anywhere

### EC2 Instance

**Logical ID:** `MyEC2Instance`

- **Type:** `AWS::EC2::Instance`
- **Properties:**
  - **InstanceType:** `t2.micro`
  - **SecurityGroups:**
    - Reference to `MySecurityGroup`
  - **ImageId:** `ami-0c11a84584d4e09dd` (Amazon Linux 2 AMI)
  - **UserData:**
    - Updates all packages
    - Installs Apache HTTP Server
    - Starts and enables Apache HTTP Server
    - Creates a simple HTML file with the content "<h1>Welcome to my website</h1>"
  - **Monitoring:** Enabled

### CloudWatch Alarm

**Logical ID:** `MyCloudWatchAlarm`

- **Type:** `AWS::CloudWatch::Alarm`
- **Properties:**
  - **AlarmDescription:** Alarm if the instance is unhealthy
  - **Namespace:** `AWS/EC2`
  - **MetricName:** `StatusCheckFailed_System`
  - **Dimensions:**
    - **Name:** `InstanceId`
    - **Value:** Reference to `MyEC2Instance`
  - **Statistic:** `Average`
  - **Period:** `30` seconds
  - **EvaluationPeriods:** `1`
  - **Threshold:** `1`
  - **ComparisonOperator:** `GreaterThanOrEqualToThreshold`
  - **AlarmActions:**
    - Automatic recovery action for the instance

## Outputs

### Website URL

**Logical ID:** `WebsiteURL`

- **Description:** URL of the website
- **Value:** Public DNS name of the EC2 instance with HTTP protocol

## Usage

To create the stack, use the AWS Management Console, AWS CLI, or any other AWS tool to deploy this CloudFormation template.

### AWS CLI Example

```sh
aws cloudformation create-stack --stack-name MySimpleWebsiteStack --template-body file://path_to_template.yaml --parameters ParameterKey=KeyName,ParameterValue=MyKeyPair
```

Replace `path_to_template.yaml` with the path to your CloudFormation template file and `MyKeyPair` with your EC2 key pair name for SSH access.

## Notes

- Ensure you have the necessary IAM permissions to create the resources specified in this template.
- The `ImageId` used in the template is for the Amazon Linux 2 AMI in the `us-east-1` region. You may need to change this ID if you are deploying in a different region.
- Adjust the security group rules as needed to match your security requirements.

```

You can download the `readme.md` file using the link below:

[Download readme.md](sandbox:/mnt/data/readme.md?_chatgptios_conversationID=957cb1fe-d699-4b46-a884-363992951507&_chatgptios_messageID=1e5bc690-8200-4f9d-9c52-09e7c0cbf1d2)
```
