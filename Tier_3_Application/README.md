# AWS Tier 3 Application Template

This AWS CloudFormation template deploys a 3-tier web application architecture, featuring a frontend, backend, and database layer. The template automates the provisioning and configuration of AWS resources to ensure a scalable, secure, and high-availability environment.

## Architecture Components

1. **Frontend Layer:**

   - **Web Server (EC2 Instance):** Hosts the web application and serves HTTP requests. Configured with a security group allowing HTTP access on port 80.

2. **Backend Layer:**

   - **Application Servers (Auto Scaling Group):** A set of EC2 instances behind an Elastic Load Balancer (ELB). The auto-scaling group ensures that the number of instances adjusts dynamically based on traffic, maintaining optimal performance.
   - **Elastic Load Balancer (ELB):** Distributes incoming traffic across the application servers to enhance availability and fault tolerance.

3. **Database Layer:**
   - **Amazon RDS (MySQL):** A managed MySQL database instance to store application data. Configured with a security group allowing access only from the application servers.

## Key Features

- **Scalability:** Utilizes an auto-scaling group for the application servers to handle varying load conditions.
- **High Availability:** The ELB distributes traffic across multiple instances and ensures the application remains accessible even if some instances fail.
- **Security:** Implements security groups to control access to each tier, ensuring only authorized traffic is allowed.
- **Automation:** Installs and configures required software on EC2 instances using user data scripts, reducing manual setup effort.

## Parameters

- `KeyName`: The name of an existing EC2 KeyPair to enable SSH access to the instances.
- `InstanceType`: The EC2 instance type for the web and application servers.
- `DBUsername`: The username for the database admin account.
- `DBPassword`: The password for the database admin account.

## Resources

- **Security Groups:** Define network access rules for the web server, application servers, and database.
- **Load Balancer:** Configured with health checks to ensure traffic is routed only to healthy instances.
- **EC2 Instances:** Provisioned for the web server and application servers with user data scripts for initial setup.
- **Auto Scaling Group:** Ensures the application servers scale based on demand.
- **RDS Instance:** A managed MySQL database instance for persistent data storage.

## Outputs

- `WebsiteURL`: The URL of the web application hosted on the provisioned infrastructure.

This template provides a robust foundation for deploying web applications, ensuring they are scalable, highly available, and secure.
