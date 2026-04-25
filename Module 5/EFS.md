#  AWS Hands-On Lab: Amazon Elastic File System (EFS)

##  Overview
This lab demonstrates how to create and use Amazon Elastic File System (EFS) to enable shared storage across multiple EC2 instances.

###  You will learn:
- Create an EFS file system
- Mount EFS on EC2 instances
- Share files between multiple servers

---

##  What is EFS?

Amazon Elastic File System (EFS) is a scalable, fully managed file storage service.

 It allows multiple EC2 instances to:
- Access the same data
- Read/write simultaneously
- Share files in real-time

---

##  Architecture Diagram

```
        +-------------------+
        |     EC2 (App1)    |
        +---------+---------+
                  |
                  |  Mount EFS
                  |
        +---------v---------+
        |       EFS         |
        | Shared Storage    |
        +---------+---------+
                  |
                  |
        +---------v---------+
        |     EC2 (App2)    |
        +-------------------+
```

---

##  Prerequisites

- AWS Account
- Basic knowledge of EC2
- 2 EC2 instances in same VPC
- Security Group allowing:
  - SSH (22)
  - NFS (2049)

---

# 🚀 Lab Steps

##  Step 1: Create EFS File System

1. Go to AWS Console → EFS
2. Click **Create File System**
3. Select your VPC
4. Keep default settings
5. Click **Create**

---

##  Step 2: Configure Security Group

Ensure EFS security group allows:

- Type: NFS
- Port: 2049
- Source: EC2 security group

---

##  Step 3: Launch EC2 Instances

- Launch **2 EC2 instances**
- Same VPC and subnet (or reachable)
- Attach same security group

---

##  Step 4: Connect to EC2

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

---

##  Step 5: Install EFS Utilities

Run on both EC2 instances:

```bash
sudo yum update -y
sudo yum install -y amazon-efs-utils
```

---

##  Step 6: Mount EFS

Create mount directory:

```bash
sudo mkdir /mnt/efs
```

Mount EFS:

```bash
sudo mount -t efs <efs-id>:/ /mnt/efs
```

 Replace `<efs-id>` with your EFS File System ID

---

##  Step 7: Test Shared Storage

### On EC2 Instance 1:

```bash
cd /mnt/efs
echo "Hello from EC2-1" > test.txt
```

---

### On EC2 Instance 2:

```bash
cat /mnt/efs/test.txt
```

###  Expected Output:

```
Hello from EC2-1
```

 This confirms shared storage is working!

---

#  Learning Outcomes

After completing this lab, you will:

- Understand EFS as shared file storage
- Mount network storage on EC2
- Share data between multiple servers
- Build scalable storage architecture

---

#  Real-World Use Cases

- Shared website files across servers
- WordPress / CMS hosting
- Container storage (EKS/ECS)
- Data analytics workloads

---

#  Cleanup (Avoid Charges)

 After completing the lab:

- Terminate EC2 instances
- Delete EFS file system

---

#  Bonus Exercises

Try extending this lab:

- Auto-mount EFS using `/etc/fstab`
- Use EFS with Auto Scaling
- Create and access files from multiple instances simultaneously

---

#  Pro Tip

EFS is:
- Elastic (auto scales storage)
- Highly available
- Managed by AWS

👉 Perfect for shared and scalable storage needs

---
