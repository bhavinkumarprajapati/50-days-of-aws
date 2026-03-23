# Day 10 -- Attach Elastic IP to EC2 (CLI Only)

## Objective

Attach an Elastic IP to an EC2 instance.

  Parameter         Value
  ----------------- ------------------
  Instance Name     nautilus-ec2
  Elastic IP Name   nautilus-ec2-eip
  Region            us-east-1

------------------------------------------------------------------------

# Concept

An **Elastic IP (EIP)** is a static public IPv4 address provided by AWS.

It is used to:

-   Maintain a fixed public IP for an instance
-   Avoid IP changes after instance restart
-   Enable stable external access

------------------------------------------------------------------------

# Solution (AWS CLI)

## Step 1 -- Get Instance ID

``` bash
aws ec2 describe-instances \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=nautilus-ec2" \
  --query "Reservations[*].Instances[*].InstanceId" \
  --output text
```

Example:

``` bash
i-01a92df6cc3bb2876
```

Set variable:

``` bash
INSTANCE_ID="i-01a92df6cc3bb2876"
```

------------------------------------------------------------------------

## Step 2 -- Get Elastic IP Allocation ID

``` bash
aws ec2 describe-addresses \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=nautilus-ec2-eip" \
  --query "Addresses[*].AllocationId" \
  --output text
```

Example:

``` bash
eipalloc-09310fa6ca50e1cfb
```

Set variable:

``` bash
ALLOCATION_ID="eipalloc-09310fa6ca50e1cfb"
```

------------------------------------------------------------------------

## Step 3 -- Associate Elastic IP

``` bash
aws ec2 associate-address \
  --region us-east-1 \
  --instance-id $INSTANCE_ID \
  --allocation-id $ALLOCATION_ID
```

Example output:

``` json
{
    "AssociationId": "eipassoc-xxxxxxxxxxxx"
}
```

------------------------------------------------------------------------

## Step 4 -- Verify Association

``` bash
aws ec2 describe-addresses \
  --region us-east-1 \
  --allocation-ids $ALLOCATION_ID
```

------------------------------------------------------------------------

# Result

Elastic IP **nautilus-ec2-eip** successfully attached to instance:

nautilus-ec2

------------------------------------------------------------------------

# Key Learnings

-   Elastic IP usage in AWS
-   Static IP management
-   Associating EIP using AWS CLI
-   Importance of stable endpoints in cloud infrastructure
