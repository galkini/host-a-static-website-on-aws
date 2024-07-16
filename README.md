![Alt text](/Host_a_Static_Website_on_AWS_mine.png)

# Static Website Hosting on AWS

This project demonstrates how to host a static HTML web application on AWS using various AWS services to ensure reliability, fault tolerance, and security. The project includes a reference architecture diagram and deployment scripts available in the [GitHub repository](https://github.com/galkini/host-a-static-website-on-aws).

## Architecture Overview

1. **VPC Configuration**: A Virtual Private Cloud (VPC) with both public and private subnets across two different availability zones was configured to provide isolation and segmentation.
2. **Internet Gateway**: Deployed an Internet Gateway to facilitate connectivity between VPC instances and the Internet.
3. **Security Groups**: Established Security Groups to act as network firewalls.
4. **Availability Zones**: Utilized two Availability Zones to enhance system reliability and fault tolerance.
5. **Public Subnets**: Leveraged Public Subnets for components such as the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint**: Implemented for secure connections to assets within both public and private subnets.
7. **Private Subnets**: Positioned web servers (EC2 instances) within Private Subnets for enhanced security.
8. **NAT Gateway**: Configured to allow instances in the private application and data subnets to access the Internet.
9. **EC2 Instances**: Hosted the static website on these instances.
10. **Application Load Balancer**: Employed to distribute web traffic evenly to an Auto Scaling Group across multiple Availability Zones.
11. **Auto Scaling Group**: Automatically managed EC2 instances to ensure availability, scalability, and elasticity.
12. **GitHub**: Used for version control and collaboration by storing web files.
13. **Certificate Manager**: Ensured secure application communications.
14. **SNS**: Configured Simple Notification Service to alert about activities within the Auto Scaling Group.
15. **Route 53**: Registered the domain name and set up a DNS record.

## Setup Instructions

The following steps outline how to deploy the static website on an EC2 instance using a bash script.

### Prerequisites

- An AWS account with necessary permissions to create VPC, subnet, EC2, security groups, etc.
- A GitHub repository containing the static website files.
- A registered domain name (optional but recommended).

### Steps to Deploy

1. **Launch an EC2 Instance**:
    - Use an AMI that supports yum (e.g., Amazon Linux 2 AMI).
    - Ensure the instance is in the public subnet and has a security group that allows HTTP traffic (port 80).

2. **Connect to the EC2 Instance**:
    - Use EC2 Instance Connect or your preferred method (SSH).

3. **Run the Initialization Script**:

    ```bash
    #!/bin/bash
    # Switch to the root user to gain full administrative privileges
    sudo su
    
    # Update all installed packages to their latest versions
    yum update -y
    
    # Install Apache HTTP Server
    yum install -y httpd
    
    # Change the current working directory to the Apache web root
    cd /var/www/html
    
    # Install Git
    yum install git -y
    
    # Clone the project GitHub repository to the current directory
    git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git
    
    # Copy all files, including hidden ones, from the cloned repository to the Apache web root
    cp -R host-a-static-website-on-aws/. /var/www/html/
    
    # Remove the cloned repository directory to clean up unnecessary files
    rm -rf host-a-static-website-on-aws
    
    # Enable the Apache HTTP Server to start automatically at system boot
    systemctl enable httpd
    
    # Start the Apache HTTP Server to serve web content
    systemctl start httpd
    ```

4. **Verify the Setup**:
    - Access the website via the public IP or DNS name of the EC2 instance.
    - Ensure that the content of the website matches that of the GitHub repository.

## Additional Configuration

1. **Domain Name and SSL**:
    - Use AWS Certificate Manager to create and manage certificates for SSL/TLS.
    - Integrate the certificate with the Application Load Balancer.
    - Configure Route 53 to route the domain name to the Load Balancer.

2. **Notifications and Monitoring**:
    - Set up Amazon SNS for notifications about auto-scaling activities.
    - Use Amazon CloudWatch for monitoring instance health and performance metrics.

## Repository

Find all configuration files, diagrams, and scripts in the [GitHub repository](https://github.com/galkini/host-a-static-website-on-aws).

## Conclusion

By following the steps outlined above, you can successfully host a static website on AWS with a robust and scalable architecture. This setup leverages various AWS services to ensure high availability, security, and efficient management of resources.
