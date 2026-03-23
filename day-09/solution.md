# Day 09 -- Enable EC2 Termination Protection (CLI Only)

## Objective

Enable termination protection for the EC2 instance:

  Parameter       Value
  --------------- ------------------------
  Instance Name   xfusion-ec2
  Feature         Termination Protection
  Region          us-east-1

------------------------------------------------------------------------

# Concept

**EC2 Termination Protection** prevents an instance from being
accidentally **terminated (deleted)**.

This is critical for:

-   Production systems\
-   Long-running workloads\
-   Preventing irreversible data loss

### Difference from Stop Protection

  Feature                  Purpose
  ------------------------ ---------------------------
  Stop Protection          Prevent stopping instance
  Termination Protection   Prevent deleting instance

------------------------------------------------------------------------

# Solution (AWS CLI)

## Step 1 -- Find Instance ID

``` bash
aws ec2 describe-instances \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=xfusion-ec2" \
  --query "Reservations[*].Instances[*].InstanceId" \
  --output text
```

Example output:

``` bash
i-xxxxxxxxxxxxxxxxx
```

Set variable:

``` bash
INSTANCE_ID="<your-instance-id>"
```

------------------------------------------------------------------------

## Step 2 -- Enable Termination Protection

``` bash
aws ec2 modify-instance-attribute \
  --region us-east-1 \
  --instance-id $INSTANCE_ID \
  --disable-api-termination "{\"Value\": true}"
```

------------------------------------------------------------------------

## Step 3 -- Verify

``` bash
aws ec2 describe-instance-attribute \
  --region us-east-1 \
  --instance-id $INSTANCE_ID \
  --attribute disableApiTermination
```

Expected output:

``` json
{
    "InstanceId": "i-xxxxxxxxxxxxxxxxx",
    "DisableApiTermination": {
        "Value": true
    }
}
```

------------------------------------------------------------------------

# Result

Termination protection has been successfully enabled for the EC2
instance:

xfusion-ec2

The instance can no longer be terminated accidentally.

------------------------------------------------------------------------

# Key Learnings

-   EC2 termination protection feature
-   Difference between stop and termination protection
-   AWS CLI instance attribute modification
-   Protecting critical infrastructure in production
