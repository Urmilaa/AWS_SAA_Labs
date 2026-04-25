#  AWS Hands-On Lab: Amazon Machine Image (AMI)

##  Overview
This lab demonstrates how to create and use an Amazon Machine Image (AMI) to build reusable, pre-configured EC2 instances.

###  You will learn:
- Launch an EC2 instance
- Install and configure software
- Create a custom AMI
- Launch a new instance using that AMI

---

##  What is an AMI?

An Amazon Machine Image (AMI) is a pre-configured template that contains:
- Operating System
- Installed software
- Application configuration

 It allows you to launch identical EC2 instances quickly and consistently.

---

##  Architecture Diagram

```
        +----------------------+
        |   Launch EC2         |
        | (Amazon Linux 2)     |
        +----------+-----------+
                   |
                   v
        +----------------------+
        | Install NGINX        |
        | Configure Server     |
        +----------+-----------+
                   |
                   v
        +----------------------+
        |   Create AMI         |
        | (Custom Image)       |
        +----------+-----------+
                   |
                   v
        +----------------------+
        | Launch New EC2       |
        | From Custom AMI      |
        +----------------------+
                   |
                   v
        +----------------------+
        | Pre-configured       |
        | Instance Ready       |
        +----------------------+
```

---

##  Prerequisites

- AWS Account
- Basic knowledge of EC2
- SSH key pair (.pem file)
- Security Group allowing:
  - SSH (22)
  - HTTP (80)

---

#  Lab Steps

##  Step 1: Launch EC2 Instance

1. Go to AWS Console → EC2 → **Launch Instance**
2. Configure:
   - AMI: Amazon Linux 2
   - Instance Type: t2.micro
   - Security Group: Allow HTTP (80)
3. Launch instance with key pair

---

##  Step 2: Connect to EC2

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

---

##  Step 3: Install and Configure NGINX

```bash
sudo yum update -y
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

###  Verify

Open browser:

```
http://<public-ip>
```

You should see the **NGINX default page**.

---

##  Step 4: Create Custom AMI

1. Go to EC2 Dashboard → Instances
2. Select your instance
3. Click:

```
Actions → Image and Templates → Create Image
```

4. Provide:
   - Name: `my-nginx-ami`
5. Click **Create Image**

---

## Step 5: Verify AMI

- Navigate to:

```
EC2 → AMIs
```

- Wait until status = **Available**

---

## Step 6: Launch EC2 from Custom AMI

1. Click **Launch Instance**
2. Select:

```
My AMIs → my-nginx-ami
```

3. Launch instance

---

##  Step 7: Validate

Open in browser:

```
http://<new-instance-public-ip>
```

### Expected Result

- NGINX is already installed  
- No manual setup required  

---

# Learning Outcomes

After completing this lab, you will:

- Understand AMI as a reusable server template
- Create custom machine images
- Launch pre-configured EC2 instances
- Reduce setup time in real-world deployments

---

#  Real-World Use Cases

- Auto Scaling Groups
- CI/CD pipelines
- Disaster recovery
- Standardized environments (Dev/Test/Prod)

---

#  Cleanup (Avoid Charges)

 Always clean up resources after the lab:

- Terminate EC2 instances
- Deregister AMI
- Delete associated snapshots

---



#  Pro Tip

AMI creation includes:
- Root volume snapshot
- Instance configuration

👉 This is why new instances come pre-installed and ready to use.

---
