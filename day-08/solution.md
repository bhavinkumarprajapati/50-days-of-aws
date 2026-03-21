# Day 08 -- Enable EC2 Stop Protection (CLI Only)

## Objective

Enable stop protection for the EC2 instance:

  Parameter       Value
  --------------- -----------------
  Instance Name   devops-ec2
  Feature         Stop Protection
  Region          us-east-1

------------------------------------------------------------------------

# Concept

**EC2 Stop Protection** prevents an instance from being accidentally
stopped via:

-   AWS Console\
-   AWS CLI\
-   API calls

It is useful for protecting critical or production systems from
unintended downtime.

------------------------------------------------------------------------

# Solution (AWS CLI)

## Step 1 -- Find Instance ID

``` bash
aws ec2 describe-instances \
  --region us-east-1 \
  --filters "Name=tag:Name,Values=devops-ec2" \
  --query "Reservations[*].Instances[*].InstanceId" \
  --output text
```

Example output:

``` bash
i-01e38c3c264bfe0b3
```

Set variable:

``` bash
INSTANCE_ID="i-01e38c3c264bfe0b3"
```

------------------------------------------------------------------------

## Step 2 -- Enable Stop Protection

``` bash
aws ec2 modify-instance-attribute \
  --region us-east-1 \
  --instance-id $INSTANCE_ID \
  --disable-api-stop "{\"Value\": true}"
```

------------------------------------------------------------------------

## Step 3 -- Verify

``` bash
aws ec2 describe-instance-attribute \
  --region us-east-1 \
  --instance-id $INSTANCE_ID \
  --attribute disableApiStop
```

Expected output:

``` json
{
    "InstanceId": "i-01e38c3c264bfe0b3",
    "DisableApiStop": {
        "Value": true
    }
}
```

------------------------------------------------------------------------

# Result

Stop protection has been successfully enabled for the EC2 instance:

devops-ec2

The instance can no longer be stopped accidentally.

------------------------------------------------------------------------

# Key Learnings

-   EC2 stop protection feature
-   Preventing accidental downtime
-   AWS CLI instance attribute modification
-   Importance of safeguards in production environments
