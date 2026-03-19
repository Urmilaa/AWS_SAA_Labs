#  AWS Hands-On Lab: EC2 Placement Groups

##  Objective

This lab helps you understand how to:

* Create EC2 Placement Groups
* Launch instances in each type
* Test performance, availability, and fault isolation

---

#  Placement Group Types

| Type      | Purpose                                  |
| --------- | ---------------------------------------- |
| Cluster   | Low latency, high throughput             |
| Spread    | High availability (fault isolation)      |
| Partition | Distributed systems / big data workloads |

---

#  Step 1: Create Placement Groups

Go to AWS Console → EC2 → Placement Groups

---

##  Create Cluster Placement Group

* Name: `cluster-pg`
* Strategy: Cluster

---

##  Create Spread Placement Group

* Name: `spread-pg`
* Strategy: Spread

---

##  Create Partition Placement Group

* Name: `partition-pg`
* Strategy: Partition
* Partitions: 2 or more

---

#  Step 2: Launch EC2 Instances

Go to EC2 → Launch Instance

---

##  Launch in Cluster Placement Group

* Launch 2 EC2 instances
* Advanced Details → Placement Group → `cluster-pg`

  Instances are placed physically close (same rack)

---

##  Launch in Spread Placement Group

* Launch 2 EC2 instances
* Select `spread-pg`

 Instances are placed on separate hardware

---

##  Launch in Partition Placement Group

* Launch 2–4 EC2 instances
* Select `partition-pg`

✔ Instances are distributed across partitions

---

#  Step 3: Verify Placement

Go to:
EC2 → Instances → Description tab

Check:

* Placement Group name
* Partition number (if applicable)

---

#  Step 4: Test Network Performance (Cluster)

SSH into instances and run:

```bash
ping <private-ip-of-other-instance>
```

✔ Expected Result:

* Very low latency

---

#  Step 5: Test Fault Isolation (Spread)

* Stop one instance
* Verify other instance is still running

 Because instances are on different hardware

---

#  Step 6: Partition Behavior

| Instance | Partition   |
| -------- | ----------- |
| EC2-1    | Partition 1 |
| EC2-2    | Partition 2 |

 Failure in one partition does not impact others

---

#  Architecture Diagrams

##  Cluster Placement Group

```
[Rack]
  EC2-1  EC2-2  EC2-3
```

---

##  Spread Placement Group

```
Rack1: EC2-1
Rack2: EC2-2
Rack3: EC2-3
```

---

##  Partition Placement Group

```
Partition 1: EC2-1, EC2-2
Partition 2: EC2-3, EC2-4
```

---

#  Important Constraints

## Cluster Placement Group

* Single Availability Zone only
* Limited instance types
* Possible capacity errors

---

## Spread Placement Group

* Maximum 7 instances per Availability Zone

---

## Partition Placement Group

* Maximum 7 partitions per Availability Zone

---

#  AWS SAA Exam Tips

* Low latency required → **Cluster Placement Group**
* High availability required → **Spread Placement Group**
* Big data workloads → **Partition Placement Group**

---

#  Common Error

###  Error:

```
Insufficient capacity error
```

###  Solution:

* Try different instance type
* Try different Availability Zone

---

#  Real-World Use Cases

| Use Case               | Placement Group |
| ---------------------- | --------------- |
| HPC / Machine Learning | Cluster         |
| Critical Applications  | Spread          |
| Hadoop / Spark         | Partition       |

---

#  Summary

* **Cluster** → High performance
* **Spread** → High availability
* **Partition** → Scalability + fault isolation

---



---
