# Day 05 – Create EBS Volume

## Objective

Create an AWS EBS volume with the following configuration:

| Parameter | Value |
|-----------|------|
| Name | xfusion-volume |
| Volume Type | gp3 |
| Size | 2 GiB |
| Region | us-east-1 |

---

# Concept

**Amazon EBS (Elastic Block Store)** provides block-level storage for EC2 instances.

It is commonly used for:

- Persistent storage
- Databases
- File systems
- Application data

## Volume Types

- **gp3** → General purpose SSD (recommended)
- io1/io2 → High-performance workloads
- st1/sc1 → Throughput optimized / cold storage

## Key Features

- Persistent storage (data remains after instance stops)
- Can be attached/detached from EC2
- Supports snapshots (backup)

---

# Solution

### Step 1 – Login to AWS Console

Select region: 
```
us-east-1
```

---

### Step 2 – Open EC2 Dashboard

Search for **EC2** and open the dashboard.

---

### Step 3 – Navigate to Volumes

From the left menu:
```
Elastic Block Store → Volumes
```

---

### Step 4 – Create Volume

Click **Create Volume** and configure:

| Field | Value |
|------|------|
| Volume Type | gp3 |
| Size | 2 GiB |
| Availability Zone | us-east-1a (or any available AZ) |

---

### Step 5 – Add Name Tag

Add a tag:

| Key | Value |
|-----|------|
| Name | xfusion-volume |

---

### Step 6 – Create Volume

Click **Create Volume**.

---



# Result

The EBS volume **xfusion-volume** has been successfully created with:

- Volume type: gp3  
- Size: 2 GiB  
- Region: us-east-1  

---

# Key Learnings

- Understanding AWS EBS volumes
- Difference between storage types
- Creating storage using AWS Console and CLI
- Importance of tagging resources

