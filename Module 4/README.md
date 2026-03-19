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
