
# CloudFormation Template: VPC with Public and Private Subnets in 3 AZs

This CloudFormation template creates a VPC in the `us-east-2` region with an internet gateway, 3 availability zones, each with a public subnet and a private subnet.

## Template Resources

- **VPC**: Creates a VPC with a CIDR block of `10.0.0.0/16`.
- **InternetGateway**: Creates an internet gateway and attaches it to the VPC.
- **PublicRouteTable**: Creates a public route table.
- **PublicRoute**: Creates a route in the public route table to the internet gateway.
- **Subnets**:
  - **PublicSubnets**: Creates three public subnets in `us-east-2a`, `us-east-2b`, and `us-east-2c` with CIDR blocks `10.0.1.0/24`, `10.0.2.0/24`, and `10.0.3.0/24` respectively.
  - **PrivateSubnets**: Creates three private subnets in `us-east-2a`, `us-east-2b`, and `us-east-2c` with CIDR blocks `10.0.101.0/24`, `10.0.102.0/24`, and `10.0.103.0/24` respectively.
- **SubnetRouteTableAssociations**: Associates each public subnet with the public route table.

## Usage

To create the stack using this template:

1. Save the CloudFormation template to a file, e.g., `vpc-template.yaml`.
2. Open the AWS Management Console.
3. Navigate to **CloudFormation**.
4. Click **Create stack**.
5. Select **With new resources (standard)**.
6. Upload the `vpc-template.yaml` file.
7. Follow the on-screen instructions to create the stack.

## Outputs

- **VPCId**: The ID of the created VPC.
- **PublicSubnets**: The IDs of the created public subnets.
- **PrivateSubnets**: The IDs of the created private subnets.

## Example

Here is an example of how to use this template with the AWS CLI:

```sh
aws cloudformation create-stack --stack-name MyVPCStack --template-body file://vpc-template.yaml
```

This command will create a CloudFormation stack named `MyVPCStack` using the `vpc-template.yaml` file.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
