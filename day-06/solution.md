# Day 05 – Create EC2 Instance

## Objective

Launch an EC2 instance with the following configuration:

| Parameter | Value |
|-----------|------|
| Instance Name | xfusion-ec2 |
| AMI | Amazon Linux |
| Instance Type | t2.micro |
| Key Pair | xfusion-kp |
| Security Group | Default |
| Region | us-east-1 |

---

# Concept

**Amazon EC2 (Elastic Compute Cloud)** provides scalable virtual servers in AWS.

It allows you to:

- Run applications on virtual machines
- Scale infrastructure easily
- Pay only for what you use

## Key Components

- **AMI (Amazon Machine Image)** → OS template
- **Instance Type** → CPU, memory, performance (t2.micro = free tier)
- **Key Pair** → SSH access
- **Security Group** → Firewall rules

---

# Solution

## Method 1 – Using AWS Console

### Step 1 – Login to AWS Console

Select region: us-east-1

---

### Step 2 – Open EC2 Dashboard

Search for **EC2** and open the dashboard.

---

### Step 3 – Launch Instance

Click **Launch Instance**.

---

### Step 4 – Configure Instance

Enter the following:

| Field | Value |
|------|------|
| Name | xfusion-ec2 |
| AMI | Amazon Linux |
| Instance Type | t2.micro |

---

### Step 5 – Create Key Pair

- Click **Create new key pair**
- Name: `xfusion-kp`
- Type: RSA
- Format: .pem
- Download the key

---

### Step 6 – Configure Network

- Select **Default VPC**
- Security Group: **Default**

---

### Step 7 – Launch Instance

Click **Launch Instance**.

---

# Method 2 – Using AWS CLI

## Create Key Pair
```
aws ec2 create-key-pair
--key-name xfusion-kp
--key-type rsa
--region us-east-1
--query 'KeyMaterial'
--output text > xfusion-kp.pem
```
Set permission:
```
chmod 400 xfusion-kp.pem
```

---

## Launch EC2 Instance

```
aws ec2 run-instances
--image-id ami-xxxxxxxx
--instance-type t2.micro
--key-name xfusion-kp
--security-groups default
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=xfusion-ec2}]'
--region us-east-1
```

> Note: Replace `ami-xxxxxxxx` with the latest Amazon Linux AMI ID for us-east-1.

---

## Verify Instance
```
aws ec2 describe-instances --region us-east-1
```

---

# Result

EC2 instance **xfusion-ec2** successfully launched with:

- Amazon Linux AMI  
- t2.micro instance type  
- RSA key pair (xfusion-kp)  
- Default security group  

---

# Key Learnings

- Launching EC2 instances
- Understanding AMI and instance types
- SSH authentication using key pairs
- Basic AWS compute setup
