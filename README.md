**# Static Website Hosting on AWS using EC2 instance, S3 bucket, Application Gateway, ACM, Route 53 and CloudWatch**
# Deploy a Static Website on AWS

## Overview

This guide outlines the steps to deploy a static website on AWS using various services like EC2, S3, IAM, Route 53, and more.

### Services Used

- **EC2 Instance**: The EC2 instance is the compute resource that runs your application or website.
- **Amazon S3 Bucket**: S3 buckets are used to store static assets such as images, videos, backups, and other files needed for the website.
- **IAM**: IAM roles and policies are used to grant necessary permissions for EC2 instances to access S3 buckets and other AWS services.
- **Route 53**: Route 53 manages DNS records, enabling you to route traffic to your load balancer.
- **Load Balancer**: A load balancer distributes incoming traffic across multiple EC2 instances to ensure high availability and reliability.

### Additional Components

- **Target Group**: Used by the load balancer to direct traffic to specific EC2 instances based on health checks and routing rules.
- **Auto Scaling Group**: Ensures that you have the right number of EC2 instances running to handle the load for your application.
- **CloudWatch**: Used for monitoring and logging, providing metrics, alarms, and logs to monitor the health and performance of your instances.
- **Launch Template**: Specifies the configuration for EC2 instances, including the AMI, instance type, key pair, security groups, and IAM roles.
- **AMI**: AMIs are used to create new EC2 instances with predefined configurations and installed software.
- **Certificate Manager**: ACM manages SSL/TLS certificates for securing your website or application traffic.

## Procedure

1. **Create an S3 Bucket**  
   Create an Amazon S3 bucket to store the static website's contents. This bucket will serve as the primary storage for all website files.

2. **Enable Public Access**  
   Enable public access to the bucket to make the website accessible to the public.

3. **Upload Website Files**  
   Upload the static website files into the newly created S3 bucket, including HTML, CSS, JavaScript, and other assets.

4. **Configure Static Website Hosting**  
   In the S3 bucket's properties, configure static website hosting by specifying the name of the landing page (e.g., `index.html`).

5. **Edit Bucket Policy**  
   Edit the bucket policy to make the bucket publicly accessible, ensuring users can access the website files stored in the S3 bucket.

6. **Create an IAM Role**  
   Create an IAM role with an S3 bucket policy to grant necessary permissions for accessing the S3 bucket.

7. **Launch EC2 Instance**  
   Launch an EC2 instance of Amazon Linux 2 and attach the previously created IAM role for accessing S3 resources.

8. **Connect to EC2 Instance**  
   Use x-shell to connect to the EC2 instance through the .pem file and switch from `ec2-user` to the root user. Install the package manager `httpd`.

9. **Install AWS CLI**  
   Install AWS CLI on the EC2 instance using:  
   ```bash
   sudo yum install aws-cli -y


