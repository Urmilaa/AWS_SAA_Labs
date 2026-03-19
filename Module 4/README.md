# AWS Hands-On Lab: Private IP vs Public IP vs Elastic IP

##  Objective

This hands-on lab helps you understand:

* Private IP
* Public IP
* Elastic IP
  using an EC2 instance in AWS.

---

#  Step 1: Launch EC2 Instance

1. Go to AWS Console → EC2
2. Click **Launch Instance**
3. Configure:

   * AMI: Amazon Linux
   * Instance Type: t2.micro
   * Network: Default VPC
   * Auto-assign Public IP: **Enable**
4. Launch instance using a key pair

---

#  Step 2: Observe IP Addresses

Go to:
EC2 Dashboard → Instances → Select your instance

You will see:

| IP Type    | Example      | Description                |
| ---------- | ------------ | -------------------------- |
| Private IP | 172.31.25.10 | Internal AWS communication |
| Public IP  | 3.110.45.67  | Internet accessible        |

---

#  Private IP (Hands-on)

### Connect to instance:

```bash
ssh -i key.pem ec2-user@<public-ip>
```

### Inside EC2:

```bash
hostname -I
```

### Output:

```
172.31.25.10
```

✔ This is your Private IP

### Key Points:

* Used within VPC
* Used for internal communication (App ↔ DB)
* Does not change

---

#  Public IP (Hands-on)

From your local machine:

```bash
ping <public-ip>
```

✔ Instance is reachable via Public IP

### Important:

* Changes after Stop → Start
* Temporary by default

---

#  Step 3: Test Public IP Change

1. Stop the instance
2. Start it again

### Observation:

| Before Restart | After Restart |
| -------------- | ------------- |
| 3.110.45.67    | 13.232.90.12  |

❗ Public IP changed

---

#  Step 4: Allocate Elastic IP

1. Go to EC2 Dashboard → Elastic IPs
2. Click **Allocate Elastic IP**
3. Click **Associate Elastic IP**
4. Select your instance

---

#  Result

| IP Type    | Example      |
| ---------- | ------------ |
| Private IP | 172.31.25.10 |
| Elastic IP | 15.206.88.20 |

---

#  Step 5: Test Elastic IP Stability

1. Stop instance
2. Start instance

### Observation:

| IP Type    | Behavior  |
| ---------- | --------- |
| Public IP  | Changes  |
| Elastic IP | Same   |

---

#  Final Understanding

##  Private IP

* Internal communication
* Used inside VPC
* Does not change

---

##  Public IP

* Used for internet access
* Temporary
* Changes on restart

---

##  Elastic IP

* Static public IP
* Does not change
* Used for production systems

---

#  Architecture Example

```
User → Elastic IP → Web Server (EC2)
                         ↓
                   Private IP
                         ↓
                   Database (RDS)
```

---

#  Bonus Test

1. Remove Elastic IP
2. Restart instance

#### You will get a new Public IP again

---

# AWS Exam Tips

* Static Public IP → Elastic IP
* Internal communication → Private IP
* Temporary internet access → Public IP

---


---

#  AWS Elastic IP – Free Tier Guide

##  Objective

Understand whether Elastic IP is free in AWS Free Tier and when charges apply.

---

#  Is Elastic IP Free in AWS?

Elastic IP is available in AWS Free Tier, but:

>  It is **NOT always free**

---

#  When Elastic IP is FREE

Elastic IP is free **only if all conditions are met:**

* It is **associated with a running EC2 instance**
* You are using **only ONE Elastic IP**

###  Example:

```
EC2 (Running) + 1 Elastic IP → FREE 
```

---

#  When Elastic IP is CHARGEABLE

You will be charged in the following scenarios:

---

##  Case 1: Elastic IP NOT Attached

```
Elastic IP allocated but NOT attached → Charge 
```

---

##  Case 2: Instance is STOPPED

```
Elastic IP attached but EC2 STOPPED → Charge 
```

---

##  Case 3: Multiple Elastic IPs

```
More than 1 Elastic IP → Extra IPs charged 
```

---

#  Why AWS Charges for Elastic IP?

* Public IPv4 addresses are **limited resources**
* AWS encourages:

  * Efficient usage
  * Avoiding unused IP allocation

---

#  AWS SAA Exam Tips

* Static Public IP needed → **Elastic IP**
* Avoid Elastic IP charges → **Attach to running instance**
* Cost optimization → **Release unused Elastic IPs**

---

#  Best Practices

After completing your lab:

1. Disassociate Elastic IP
2. Release Elastic IP

> ✔ Prevents unnecessary charges

---

#  Pro Tip

Instead of Elastic IP in production, consider:

* Elastic Load Balancer (ELB)
* Amazon Route 53 (DNS)

### Benefits:

* High availability
* Scalability
* No manual IP management

---

#  Summary

| Scenario                             | Cost      |
| ------------------------------------ | --------- |
| 1 Elastic IP + Running EC2           | Free     |
| Elastic IP not attached              | Charged  |
| EC2 stopped with Elastic IP attached | Charged  |
| Multiple Elastic IPs                 | Charged  |

---

