# Day 01 – Solution

## Concept

An AWS **Key Pair** is used to securely connect to EC2 instances using SSH.

A key pair consists of:

- **Public Key** – Stored by AWS and associated with an EC2 instance
- **Private Key (.pem)** – Downloaded and stored locally, used to connect to the instance

Example SSH command:

ssh -i xfusion-kp.pem ec2-user@public-ip

---

## Steps (AWS Console)

1. Log in to the AWS Console.
2. Navigate to **EC2 Dashboard**.
3. In the left sidebar, select:

Network & Security → Key Pairs

4. Click **Create Key Pair**.

5. Enter the following configuration:

Name: xfusion-kp  
Key pair type: RSA  

6. Click **Create Key Pair**.

7. The file **xfusion-kp.pem** will automatically download.

---

## Key Learnings

- AWS EC2 authentication using key pairs
- Difference between public and private keys
