
##  Lab 1: Launch EC2 with Hibernate Enabled

###  Objective

Launch an EC2 instance with hibernation enabled

###  Steps

1. Go to EC2 Dashboard
2. Click **Launch Instance**
3. Configure:

   * AMI: Ubuntu / Amazon Linux
   * Instance Type: `t3.micro`
4. Expand **Advanced Details**
5. Enable:

   ```
   Enable hibernation
   ```
6. Configure storage (ensure enough EBS volume)
7. Launch instance

###  Verification

* Go to **Instance → Actions → Instance State**
* Hibernate option should be available

---

##  Lab 2: Create In-Memory Data

###  Objective

Simulate RAM usage to test hibernation

###  Steps

SSH into instance:

```bash
sudo apt update -y
sudo apt install stress -y

# Simulate memory usage
stress --vm 1 --vm-bytes 300M --timeout 300s &
```

Create a background process:

```bash
sleep 5000 &
ps aux | grep sleep
```

###  Verification

* Note the PID of sleep process

---

##  Lab 3: Hibernate the Instance

###  Objective

Put EC2 into hibernation state

###  Steps

1. Go to EC2 Console
2. Select instance
3. Click:

   ```
   Actions → Instance State → Hibernate
   ```

###  What Happens Internally

* RAM is saved to EBS root volume
* Instance stops
* No compute charges (only EBS billed)

---

##  Lab 4: Resume Instance

###  Objective

Validate session persistence

###  Steps

1. Start the instance
2. SSH into it

###  Verification

```bash
ps aux | grep sleep
ps aux | grep stress
```

If processes exist → Hibernate worked

---


## Lab 5: Compare Stop vs Hibernate

###  Objective

Understand key differences

###  Steps

#### Step 1: Stop Instance

```
Actions → Instance State → Stop
```

Start again and check:

```bash
ps aux | grep sleep
```

Process will NOT exist

#### Step 2: Hibernate Instance

Repeat hibernate → start → check

Process SHOULD exist

---

##  Lab 6: Failure Scenarios

###  Objective

Understand when Hibernate fails

###  Try these:

* Use unsupported instance type (e.g., t2.micro)
* Disable hibernation at launch
* Use insufficient EBS volume
* Use instance store-backed AMI

###  Expected Result

* Hibernate option not available

---

##  Lab 7: Real-World Use Case

###  Scenario

* Long-running application in memory
* Needs fast restart
* Cannot reinitialize

###  Solution

Use EC2 Hibernate

---

##  Exam Tips (Important)

###  When to Use Hibernate

* Preserve in-memory state
* Faster startup than stop/start
* Resume applications instantly

###  Constraints

* Supported instance families only
* Must be enabled at launch
* Root volume must be EBS
* RAM must fit in EBS volume

---


