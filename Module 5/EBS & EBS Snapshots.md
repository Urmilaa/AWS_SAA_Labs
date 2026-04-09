# 🧪 AWS Hands-On Lab: EBS Volume & Snapshot

## 📌 Overview

This hands-on lab demonstrates how to create, attach, and manage an EBS volume, along with creating and restoring snapshots. It is designed for practical learning and exam preparation.

---

## 🎯 Objectives

By completing this lab, you will:

* Launch an EC2 instance
* Create an EBS volume
* Attach and mount the volume
* Write data to the volume
* Create an EBS snapshot
* Restore a new volume from snapshot

---

## 🧱 Prerequisites

* AWS Account
* Basic knowledge of EC2 and SSH
* Key pair for EC2 access

---

## 🚀 Step 1: Launch EC2 Instance

1. Navigate to EC2 Console
2. Click **Launch Instance**
3. Configure:

   * Name: `ebs-lab-instance`
   * AMI: Ubuntu
   * Instance Type: `t3.small`
   * Key Pair: Select/Create
4. Launch the instance

> ⚠️ Note: Record the Availability Zone (AZ)

---

## 💾 Step 2: Create EBS Volume

1. Go to **EC2 → Volumes**
2. Click **Create Volume**
3. Configure:

   * Volume Type: `gp3`
   * Size: `5 GiB`
   * AZ: Same as EC2 instance
4. Click **Create Volume**

> ✅ EBS volumes must be in the same AZ as the EC2 instance

---

## 🔗 Step 3: Attach Volume

1. Select the created volume
2. Click **Actions → Attach Volume**
3. Select EC2 instance
4. Device name:

   ```bash
   /dev/xvdf
   ```

---

## 🔐 Step 4: Connect to EC2

```bash
ssh -i your-key.pem ec2-user@<public-ip>
```

---

## 🔍 Step 5: Verify Volume

```bash
lsblk
```

---

## 🧰 Step 6: Format Volume

```bash
sudo mkfs -t ext4 /dev/xvdf
```

---

## 📂 Step 7: Mount Volume

```bash
sudo mkdir /data
sudo mount /dev/xvdf /data
```

Verify:

```bash
df -h
```

---

## ✍️ Step 8: Write Data

```bash
echo "EBS Snapshot Demo" | sudo tee /data/demo.txt
cat /data/demo.txt
```

---

## 📸 Step 9: Create Snapshot

1. Go to **EC2 → Volumes**
2. Select volume
3. Click **Actions → Create Snapshot**
4. Name: `ebs-lab-snapshot`
5. Click **Create Snapshot**

> 📌 Snapshots are incremental backups

---

## 🔄 Step 10: Create Volume from Snapshot

1. Go to **Snapshots**
2. Select snapshot
3. Click **Actions → Create Volume**
4. Configure:

   * Volume Type: gp3
   * AZ: Same or different

---

## 🔗 Step 11: Attach New Volume

* Attach to same EC2 instance
* Device name:

```bash
/dev/xvdg
```

---

## 📂 Step 12: Mount New Volume

```bash
lsblk
sudo mkdir /data2
sudo mount /dev/xvdg /data2
```

---

## ✅ Step 13: Verify Data

```bash
cat /data2/demo.txt
```

Expected Output:

```bash
EBS Snapshot Demo
```

---

## ⚠️ Troubleshooting

| Issue              | Solution                  |
| ------------------ | ------------------------- |
| Volume not visible | Run `lsblk` or reattach   |
| Mount fails        | Check correct device name |
| Data missing       | Verify correct mount path |

---

## 🧠 Key Learnings

* EBS volumes are AZ-specific
* Snapshots are region-level
* Snapshots are incremental
* Restoring snapshot creates a new volume

---

## 🧹 Cleanup

To avoid charges:

* Terminate EC2 instance
* Delete EBS volumes
* Delete snapshots

---

## 📚 Use Cases

* Backup and restore
* Disaster recovery
* Data migration across AZs

---

## ⭐ Contributing

Feel free to fork this repository and enhance the lab with automation scripts (CLI/Terraform).

---

## 📄 License

This project is for educational purposes.
