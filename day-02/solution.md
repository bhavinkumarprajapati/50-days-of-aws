# Day 02 -- Create Security Group

## Objective

Create an AWS Security Group with the following configuration:

  Parameter     Value
  ------------- -----------------------------------------
  Name          devops-sg
  Description   Security group for Nautilus App Servers
  VPC           Default VPC
  Region        us-east-1

Inbound rules:

  Type   Port   Source
  ------ ------ -----------
  HTTP   80     0.0.0.0/0
  SSH    22     0.0.0.0/0

Outbound rule:

  Type          Destination
  ------------- -------------
  All Traffic   0.0.0.0/0

------------------------------------------------------------------------

# Concept

An **AWS Security Group** acts as a **virtual firewall** that controls
inbound and outbound traffic for EC2 instances.

Security groups operate at the **instance level** and only allow traffic
that matches the defined rules.

### Important Characteristics

-   Security groups are **stateful**
-   Only **allow rules** exist (no deny rules)
-   Rules apply to **EC2 instances attached to the security group**

Example:

If HTTP (80) is allowed, users can access the application running on the
server via a web browser.

------------------------------------------------------------------------

# Solution

## Method 1 -- Using AWS Console

### Step 1 -- Login to AWS Console

Login using the provided credentials and select the region:

    us-east-1

### Step 2 -- Open EC2 Dashboard

Search for **EC2** in AWS services and open the **EC2 Dashboard**.

### Step 3 -- Navigate to Security Groups

From the left menu:

    Network & Security → Security Groups

### Step 4 -- Create Security Group

Click **Create security group**.

Enter the following values:

  Field                 Value
  --------------------- -----------------------------------------
  Security Group Name   devops-sg
  Description           Security group for Nautilus App Servers
  VPC                   Default VPC

### Step 5 -- Add Inbound Rules

Add the following rules:

  Type   Protocol   Port   Source
  ------ ---------- ------ -----------
  HTTP   TCP        80     0.0.0.0/0
  SSH    TCP        22     0.0.0.0/0

### Step 6 -- Configure Outbound Rules

Keep the **default outbound rule**:

  Type          Protocol   Port   Destination
  ------------- ---------- ------ -------------
  All Traffic   All        All    0.0.0.0/0

### Step 7 -- Create Security Group

Click **Create security group**.

------------------------------------------------------------------------

# Method 2 -- Using AWS CLI

### Create Security Group

    aws ec2 create-security-group \
    --group-name devops-sg \
    --description "Security group for Nautilus App Servers" \
    --region us-east-1

### Add HTTP Rule

    aws ec2 authorize-security-group-ingress \
    --group-name devops-sg \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0 \
    --region us-east-1

### Add SSH Rule

    aws ec2 authorize-security-group-ingress \
    --group-name devops-sg \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0 \
    --region us-east-1

### Verify Security Group

    aws ec2 describe-security-groups \
    --group-names devops-sg \
    --region us-east-1

------------------------------------------------------------------------

# Result

The Security Group **devops-sg** was successfully created in the
**default VPC** with the following access rules:

-   HTTP (80) allowed from anywhere
-   SSH (22) allowed from anywhere
-   All outbound traffic allowed

------------------------------------------------------------------------

# Key Learnings

-   AWS Security Groups function as instance-level firewalls
-   Inbound rules control incoming traffic
-   Outbound rules control outgoing traffic
-   Security groups are stateful, meaning return traffic is
    automatically allowed