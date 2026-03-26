# Day 11 – Attach Elastic Network Interface to EC2 (CLI Only)

## Objective

Attach an Elastic Network Interface (ENI) to an EC2 instance.

| Parameter     | Value        |
|---------------|--------------|
| Instance Name | xfusion-ec2  |
| ENI Name      | xfusion-eni  |
| Region        | us-east-1    |

---

# Concept

An **Elastic Network Interface (ENI)** is a virtual network card in AWS that can be attached to EC2 instances.

It is used to:
- Add secondary network interfaces to an instance
- Enable dual-homed instances (multiple subnets/IPs)
- Move network interfaces between instances for failover
- Maintain a fixed MAC address independent of the instance lifecycle

---

# Solution (AWS CLI)

## Step 1 – Get Instance ID

```bash
aws ec2 describe-instances \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=xfusion-ec2" \
  --query "Reservations[0].Instances[0].InstanceId" \
  --output text
```

Example output:

```
i-028cbc3fcc033669a
```

Set variable:

```bash
INSTANCE_ID="i-028cbc3fcc033669a"
```

---

## Step 2 – Get ENI ID

```bash
aws ec2 describe-network-interfaces \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=xfusion-eni" \
  --query "NetworkInterfaces[0].NetworkInterfaceId" \
  --output text
```

Example output:

```
eni-0cb1571a6b92182d7
```

Set variable:

```bash
ENI_ID="eni-0cb1571a6b92182d7"
```

---

## Step 3 – Check Instance Initialisation Status

Wait until instance status checks return `ok` before attaching:

```bash
aws ec2 describe-instance-status \
  --region us-east-1 \
  --instance-ids $INSTANCE_ID \
  --query "InstanceStatuses[0].InstanceStatus.Status" \
  --output text
```

Expected output:

```
ok
```

---

## Step 4 – Attach the ENI

```bash
aws ec2 attach-network-interface \
  --region us-east-1 \
  --network-interface-id $ENI_ID \
  --instance-id $INSTANCE_ID \
  --device-index 1
```

> `--device-index 1` is used because index `0` is already occupied by the primary network interface.

Example output:

```json
{
    "AttachmentId": "eni-attach-0xxxxxxxxxxxxxxxxx"
}
```

---

## Step 5 – Verify Attachment Status

```bash
aws ec2 describe-network-interfaces \
  --region us-east-1 \
  --network-interface-ids $ENI_ID \
  --query "NetworkInterfaces[0].Attachment.Status" \
  --output text
```

Expected output:

```
attached
```

---

# Result

ENI **xfusion-eni** successfully attached to instance **xfusion-ec2** with status `attached`.

---

# Key Learnings

- What an Elastic Network Interface (ENI) is and its use cases
- How to look up EC2 instance and ENI IDs using AWS CLI filters
- Correct parameter order when calling `attach-network-interface`
- Importance of waiting for instance initialisation before making changes
- How to verify attachment status via `describe-network-interfaces`
