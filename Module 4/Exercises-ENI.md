#  AWS ENI (Elastic Network Interface) Hands-On Labs
---

##  Overview

This repository contains **hands-on lab exercises** to understand **Elastic Network Interfaces (ENI)** in AWS. 
---

---

##  Lab 1: Create and Attach an ENI

### Objective

Create an ENI and attach it to an EC2 instance.

### Steps

1. Navigate to **EC2 → Network Interfaces**

2. Click **Create Network Interface**

3. Choose:

   * Subnet (same as EC2)
   * Security Group (allow SSH)

4. Launch EC2 instance (Amazon Linux)

5. Attach ENI:

   ```
   EC2 → Instances → Select Instance → Actions → Networking → Attach Network Interface
   ```

6. SSH into instance:

   ```
   ip addr
   ```

### Expected Output

* Additional network interface (`eth1`) visible

---

##  Lab 2: ENI Detach & Reattach (Failover Simulation)

### Objective

Simulate failover by moving ENI between instances.

### Steps

1. Launch **two EC2 instances** (same AZ)

2. Attach ENI to Instance-1

3. Verify:

   ```
   ip addr
   ```

4. Detach ENI:

   ```
   EC2 → Network Interfaces → Detach
   ```

5. Attach ENI to Instance-2

6. Verify again:

   ```
   ip addr
   ```

### Key Learning

* ENI retains:

  * Private IP
  * Security Groups
  * MAC Address

---

##  Lab 3: Multiple Private IPs on ENI

### Objective

Assign multiple private IPs to a single ENI.

### Steps

1. Go to ENI → **Manage IP Addresses**

2. Add secondary private IPs

3. SSH into instance:

   ```
   ip addr
   ```

4. Bind service:

   ```
   sudo python3 -m http.server 80 --bind <secondary-ip>
   ```

### Use Case

* Hosting multiple services/websites

---

##  Lab 4: Associate Elastic IP with ENI

### Objective

Understand how Elastic IP works with ENI.

### Steps

1. Allocate Elastic IP

2. Associate it with ENI (not instance)

3. Test:

   ```
   curl http://<Elastic-IP>
   ```

### Key Learning

* Elastic IP is attached to ENI, not instance

---

##  Lab 5: ENI & Security Groups

### Objective

Understand how security groups are applied.

### Steps

1. Create two security groups:

   * SG-1 → Allow SSH
   * SG-2 → Deny SSH

2. Attach SG-1 → Test SSH (should work)

3. Switch to SG-2 → Test SSH (should fail)

### Key Learning

* Security groups are attached at ENI level

---

##  Lab 6: ENI Subnet/AZ Limitation

### Objective

Understand ENI placement restrictions.

### Steps

1. Create ENI in Subnet-A
2. Try attaching it to instance in Subnet-B (different AZ)

### Expected Result

* Operation fails

### Key Learning

* ENI must be in same Availability Zone

---

##  Lab 7: Source/Destination Check

### Objective

Configure ENI for NAT/Bastion use case.

### Steps

1. Select EC2 instance
2. Disable source/destination check:

   ```
   EC2 → Instance → Networking → Change Source/Destination Check → Disable
   ```

### Use Case

* NAT Instance
* Traffic routing

---

##  Lab 8: ENI Limits per Instance Type

### Objective

Understand scaling limits.

### Steps

1. Use small instance (e.g., t2.micro)
2. Try attaching multiple ENIs

### Expected Result

* Limited ENIs allowed

### Key Learning

* ENI limit depends on instance type

---



✨ Happy Learning & Best of Luck for your AWS SAA Exam!
