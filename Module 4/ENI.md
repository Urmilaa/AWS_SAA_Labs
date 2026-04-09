#  AWS Hands-On Lab: Elastic Network Interfaces (ENI)

##  Objective

This lab helps you understand how to:

* Create an Elastic Network Interface (ENI)
* Attach/detach ENI to EC2 instances
* Assign multiple private IPs
* Simulate failover using ENI

---

#  What is ENI?

An **Elastic Network Interface (ENI)** is a virtual network card in AWS that can be attached to an EC2 instance.

### It contains:

* Private IP address
* Security Groups
* MAC address  - A MAC (Media Access Control) address is a unique 12-character hexadecimal identifier (e.g., 1A:2B:3C:4D:5E:6F) permanently assigned to a device's network interface controller (NIC) by the manufacturer. It serves as a physical, hardware-level address used to identify and manage devices on a local network, such as Wi-Fi or Ethernet

---

#  Step 1: Launch EC2 Instance

1. Go to AWS Console → EC2
2. Click **Launch Instance**
3. Configure:

   * AMI: ubuntu
   * Instance type: t3.small
   * VPC: Default
4. Launch instance

---

#  Step 2: Observe Default ENI

Go to:
EC2 → Instances → Select your instance

Click:
**Networking tab**

 You will see:

* Primary ENI (`eth0`)
* Private IP assigned

---

#  Step 3: Create a New ENI

Go to:
EC2 → **Network Interfaces**

1. Click **Create Network Interface**
2. Configure:

   * Subnet: Same as EC2 instance
   * Security Group: Default
3. Click **Create**

---

#  Step 4: Attach ENI to EC2

1. Select the ENI
2. Click **Actions → Attach**
3. Choose your EC2 instance
4. Device Index:

   * `1` (secondary interface)

 Now your instance has:

* `eth0` (primary ENI)
* `eth1` (secondary ENI)

---

#  Step 5: Verify Inside EC2

SSH into instance:

```bash
ip addr
```

 Output shows:

* eth0 → primary ENI
* eth1 → secondary ENI

---

#  Step 6: Assign Secondary Private IP

1. Go to ENI → Select ENI
2. Click **Manage IP addresses**
3. Click **Assign new private IP**

---

#  Verify:

Inside EC2:

```bash
ip addr
```

 Multiple IPs visible on same ENI

---

#  Step 7: Detach & Reattach ENI

1. Detach ENI from EC2-1
2. Attach ENI to EC2-2

---

#  Result

✔ Same:

* Private IP
* MAC address
* Security group

 Moves with ENI

---

#  Step 8: Failover Simulation

### Scenario:

* EC2-1 fails
* Move ENI to EC2-2

 Traffic resumes using same IP

---

#  Architecture

```
Before:
EC2-1 → ENI (IP: 172.31.10.5)

After Failover:
EC2-2 → ENI (Same IP: 172.31.10.5)
```

---

#  Important Constraints

* ENI must be in the **same Availability Zone**
* Number of ENIs depends on instance type
* `eth0` is always primary interface

---

#  AWS SAA Exam Tips

* Move network identity → **ENI**
* Multiple IPs on instance → **ENI**
* Failover with same private IP → **ENI**

---

#  Real-World Use Cases

| Use Case           | Benefit                   |
| ------------------ | ------------------------- |
| Failover           | Quick recovery            |
| Multi-homing       | Multiple networks         |
| Security isolation | Different security groups |

---

#  Summary

* ENI = Portable network interface
* Supports multiple IPs
* Can be detached & reattached
* Enables fast failover

---


