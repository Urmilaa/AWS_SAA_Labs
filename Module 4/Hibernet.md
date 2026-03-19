#  AWS Hands-On Lab: Implementing EC2 Hibernate

##  Objective

This lab helps you understand how to:

* Enable Hibernate for an EC2 instance
* Hibernate and resume the instance
* Verify memory (RAM) state persistence

---

#  What is EC2 Hibernate?

EC2 Hibernate allows you to **pause an instance by saving its RAM state to EBS**.

### ✔ Key Benefits:

* Resume applications exactly where they stopped
* Faster startup compared to normal stop
* Useful for long-running workloads

---

#  Prerequisites

Before starting, ensure:

* Instance type supports Hibernate
* Root volume must be **EBS (not instance store)**
* Root volume size ≥ RAM size
* Supported OS (Amazon Linux 2 / Ubuntu)
* EBS encryption enabled (recommended/required in many cases)

---

#  Step 1: Launch EC2 with Hibernate Enabled

1. Go to AWS Console → EC2
2. Click **Launch Instance**

### Configure:

* AMI: Amazon Linux 2 / Ubuntu
* Instance type: t2.micro
* Storage: Increase size if required

---

##  Enable Hibernate

In **Advanced Details**:

* Enable:  **Allow Hibernate**

---

##  Storage Requirement Example

```
t2.micro → 1 GB RAM → allocate at least 2–3 GB EBS
```

---

#   Step 2: Connect to Instance

```bash id="jrlpr5"
ssh -i key.pem ec2-user@<public-ip>
```

---

# 🧪 Step 3: Create Test Data

Inside EC2:

```bash id="e82uev"
echo "Hibernate Test Running" > test.txt
```

Start a running process:

```bash id="ilc4py"
top
```

---

#  Step 4: Hibernate the Instance

1. Go to EC2 → Instances
2. Select your instance
3. Click: **Instance State → Stop**

> Since Hibernate is enabled, instance will enter **Hibernated state**

---

#  Step 5: Verify State

* Instance state will show:

  ```
  Stopped (Hibernated)
  ```

---

#  Step 6: Start Instance Again

1. Click **Start Instance**

---

#  Step 7: Verify Resume Behavior

Reconnect:

```bash id="3e4tpf"
ssh -i key.pem ec2-user@<public-ip>
```

---

##  Verify File

```bash id="6is0b4"
cat test.txt
```

✔ File should still exist

---

##  Verify Process

* Previously running processes (like `top`) resume
* System continues from previous state

---

#  How Hibernate Works

```
Before Hibernate:
RAM → Running applications

During Hibernate:
RAM → Saved to EBS

After Restart:
EBS → Restored to RAM
```

---

#  Common Issues

##  Hibernate Option Not Available

* Unsupported instance type
* Root volume not EBS
* Encryption not enabled

---

##  Instance Stops Normally (No Hibernate)

* Hibernate not enabled during launch
* Insufficient EBS space

---

#  Hibernate vs Stop vs Terminate

| Action    | RAM Saved | Data Saved (EBS) | Billing    |
| --------- | --------- | ---------------- | ---------- |
| Stop      | ❌         | ✅                | No compute |
| Hibernate | ✅         | ✅                | No compute |
| Terminate | ❌         | ❌                | Deleted    |

---

#  AWS SAA Exam Tips

* Resume application quickly → **Hibernate**
* Preserve RAM state → **Hibernate**
* Cold start acceptable → **Stop**

---

#  Real-World Use Cases

| Use Case          | Benefit                    |
| ----------------- | -------------------------- |
| Long-running apps | Resume quickly             |
| Dev/Test          | Save application state     |
| Cost optimization | Stop without losing memory |

---

# Summary

* Hibernate saves RAM to EBS
* Faster restart than Stop
* Requires proper configuration
* Ideal for stateful workloads

---


