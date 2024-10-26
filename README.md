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
   Synchronize S3 Bucket with EC2
Synchronize the contents of the S3 bucket with the EC2 instance's /var/www/html directory using:

bash
Copy code
aws s3 ls s3://sample-website-aws-bucket
Start the Web Server
Start the server on the EC2 instance using:

bash
Copy code
systemctl start httpd
Enable Server on Boot
Enable the server to start on boot with:

bash
Copy code
systemctl enable httpd
Create a Hosted Zone in Route 53
Create a hosted zone in Route 53 for the domain name associated with the website.

Update Domain Nameservers
Update the nameservers of the domain's hosting website with the nameservers provided by AWS to direct traffic to AWS.

Request SSL Certificate
Request a certificate from ACM for the domain name studentgulmohurschool.online to enable HTTPS encryption.

Request Wildcard SSL Certificate
Request another certificate for *.studentgulmohurschool.online to include all subdomains.

Create CNAME Record
Create a CNAME record in Route 53 to map the domain to the load balancer's DNS name for better traffic management.

Create Target Group
Create a target group for managing traffic distribution.

Create and Configure Load Balancer
Create a load balancer and attach it to the target group. The ACM certificate should also be attached, and both HTTP and HTTPS listeners should be configured.

Redirect HTTP to HTTPS
Redirect HTTP requests to HTTPS for secure access.

Add SSL Certificate for Subdomains
Add the SSL certificate for subdomains to the HTTPS port to ensure secure connections for subdomains.

Create Route 53 Records
Create records in Route 53 for both the root domain and subdomains to distribute the load, choosing alias as Classic and Application Load Balancer.

Access the Website
Access the website using the root domain (studentgulmohurschool.online) and sub-domain (mail.studentgulmohurschool.online). Both connections should be secured.

Create an Image for Auto Scaling Group
Create an image of the EC2 instance where the web server is configured. A template should be launched from this image.

Launch Template
Create a new security group for the template, allowing HTTP, HTTPS, and SSH. An auto-scaling group should then be created from the template, selecting all availability zones and attaching it to the load balancer with the target group.

Create an Auto Scaling Group
Create an auto-scaling group with all availability zones and attach it to the load balancer.

Set Scaling Policies
Set the desired, minimum, and maximum capacity for the servers. Launch a new instance from the template, which will appear in the EC2 dashboard.

Create CloudWatch Alarms
Create CloudWatch alarms based on the CPU utilization metrics of the instances. If CPU utilization is >= 50%, new instances should be launched; if < 40%, instances should be terminated to meet the desired and minimum capacity.

Attach Policies to Auto Scaling Group
Create two dynamic policies in the auto-scaling group and attach them to the CloudWatch alarms.

Monitor Auto Scaling
Based on CPU utilization, the alarms should change from Insufficient data → OK → In-alarm state. New instances will be added and removed as per the CPU utilization.

Problems Encountered
The SSL certificate was initially issued only for *.studentgulmohurschool.online, covering all subdomains but not the root domain studentgulmohurschool.online.
Troubleshooting
Requested a new SSL certificate covering both the root domain and its subdomains. After obtaining the updated SSL certificate, the website became securely accessible via HTTPS, and HTTP requests were automatically redirected to HTTPS. An A-type record was created in Route 53 to associate both the root domain and subdomains with the alias as Application and Classic Load Balancer.
Conclusion
By following these detailed steps, you can deploy a static website on AWS, ensuring it is secure, scalable, and efficiently managed. This comprehensive guide should help you navigate through the process seamlessly.


