**AWS Hands-On Lab: Amazon Machine Image (AMI)**
Overview

This lab demonstrates how to create and use an Amazon Machine Image (AMI) to build reusable, pre-configured EC2 instances.

You will:

Launch an EC2 instance
Install and configure software
Create a custom AMI
Launch a new instance using that AMI
 **What is an AMI**

An Amazon Machine Image (AMI) is a pre-configured template that contains:

Operating System
Installed software
Application configuration

 It allows you to launch identical EC2 instances quickly and consistently.

 **Architecture Diagram**
 Workflow
[Launch EC2] 
      ↓
[Install NGINX / Customize]
      ↓
[Create AMI]
      ↓
[Launch New EC2 from AMI]
**Prerequisites**
AWS Account
Basic knowledge of EC2
SSH key pair (.pem file)
Security Group allowing:
SSH (22)
HTTP (80)
 **Lab Steps**
**Step 1:** Launch EC2 Instance
Go to AWS Console → EC2 → Launch Instance
**Configure:**
AMI: Amazon Linux 2
Instance Type: t2.micro
Security Group: Allow HTTP (80)
Launch instance with key pair
 **Step 2**: Connect to EC2
ssh -i your-key.pem ec2-user@<public-ip>
**Step 3**: Install and Configure NGINX
sudo yum update -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
**Verify**

**Open browser:**

http://<public-ip>

You should see the NGINX default page.

**Step 4** Create Custom AMI
Go to EC2 Dashboard → Instances
Select your instance

Click:

Actions → Image and Templates → Create Image
Provide:
Name: my-nginx-ami
Click Create Image
**Step 5**: Verify AMI

Navigate to:

EC2 → AMIs
Wait until status = Available
 **Step 6**: Launch EC2 from Custom AMI
Click Launch Instance

Select:

My AMIs → my-nginx-ami
Launch instance

**Step 7**: Validate

Open in browser:

http://<new-instance-public-ip>
 **Expected Result**
NGINX is already installed
No manual setup required

**Real-World Use Cases**
Auto Scaling Groups
CI/CD pipelines
Disaster recovery
Standardized environments (Dev/Test/Prod)
 **Cleanup (Avoid Charges)**

 **Always clean up resources after the lab:**

Terminate EC2 instances
Deregister AMI
Delete associated snapshots
