# AWS CloudFormation Template for Simple Website on EC2

This CloudFormation template sets up a simple website running on an EC2 instance. The stack includes a security group, an EC2 instance, and a basic user data script to install a web server.

## Template Details

### Security Group

- Allows HTTP access on port 80 from anywhere.

### EC2 Instance

- Instance Type: `t2.micro`
- AMI: Amazon Linux 2 (AMI ID: `ami-0c11a84584d4e09dd`, replace with the region-specific AMI ID)
- User Data Script:
  - Updates the instance
  - Installs the Apache web server
  - Starts the Apache web server
  - Enables the Apache web server to start on boot
  - Creates a simple HTML file at `/var/www/html/index.html`

### Outputs

- **WebsiteURL**: Provides the URL of the website using the public DNS name of the EC2 instance.

## Steps to Use

1. Save the provided CloudFormation template to a file, e.g., `ec2-website.yaml`.
2. In the AWS Management Console, navigate to the CloudFormation service.
3. Create a new stack by uploading the `ec2-website.yaml` file.
4. Follow the prompts to create the stack.
5. Once the stack is created, check the Outputs section of the CloudFormation stack for the URL to access the website.

## Template

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: AWS CloudFormation Template to deploy a simple website on EC2

Resources:
  # Security Group
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable HTTP access on port 80
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: "80"
          ToPort: "80"
          CidrIp: 0.0.0.0/0

  # EC2 Instance
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      SecurityGroups:
        - Ref: MySecurityGroup
      ImageId: ami-0c55b159cbfafe1f0 # Amazon Linux 2 AMI (use a region-specific AMI ID)
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          yum update -y
          yum install -y httpd
          systemctl start httpd
          systemctl enable httpd
          echo "<h1>Welcome to my website</h1>" > /var/www/html/index.html

Outputs:
  WebsiteURL:
    Description: URL of the website
    Value:
      Fn::Sub: http://${MyEC2Instance.PublicDnsName}
```

## Notes

- Replace the AMI ID (`ami-0c11a84584d4e09dd`) with the appropriate ID for your region.
- This template does not include an EC2 key pair configuration, so SSH access is not enabled by default.
- Make sure to properly manage the instance, as direct SSH access is not configured.
