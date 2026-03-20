# Day 06 -- Modify EC2 Instance Type

## Objective

Modify the EC2 instance **xfusion-ec2**:

-   Change instance type: **t2.micro → t2.nano**
-   Ensure status checks are completed before modification
-   Ensure instance is in **running state** after change

------------------------------------------------------------------------

# Concept

EC2 instance types define the compute resources (CPU, memory).

Changing instance type is used for:

-   Cost optimization
-   Right-sizing infrastructure
-   Performance tuning

## Important Rules

-   Instance must be **stopped** before changing type
-   Status checks must be **2/2 (ok)**
-   Instance must be **started again after modification**

------------------------------------------------------------------------

# Solution (AWS CLI -- DevOps Approach)

## Step 1 -- Find the Instance

``` bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=xfusion-ec2" \
  --query "Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType,Tags[?Key=='Name'].Value|[0]]" \
  --output table
```

Set instance id:

``` bash
INSTANCE_ID="<your-instance-id>"
```

------------------------------------------------------------------------

## Step 2 -- Check Status

``` bash
aws ec2 describe-instance-status \
  --instance-ids $INSTANCE_ID \
  --query "InstanceStatuses[*].[InstanceId,SystemStatus.Status,InstanceStatus.Status]" \
  --output table
```

------------------------------------------------------------------------

## Step 3 -- Wait for Status Checks

``` bash
aws ec2 wait instance-status-ok --instance-ids $INSTANCE_ID
echo "Status checks passed!"
```

------------------------------------------------------------------------

## Step 4 -- Stop Instance

``` bash
aws ec2 stop-instances --instance-ids $INSTANCE_ID
aws ec2 wait instance-stopped --instance-ids $INSTANCE_ID
echo "Instance stopped."
```

------------------------------------------------------------------------

## Step 5 -- Change Instance Type

``` bash
aws ec2 modify-instance-attribute \
  --instance-id $INSTANCE_ID \
  --instance-type "{\"Value\": \"t2.nano\"}"
```

------------------------------------------------------------------------

## Step 6 -- Start Instance

``` bash
aws ec2 start-instances --instance-ids $INSTANCE_ID
aws ec2 wait instance-running --instance-ids $INSTANCE_ID
echo "Instance is now running!"
```

------------------------------------------------------------------------

## Step 7 -- Verify Changes

``` bash
aws ec2 describe-instances \
  --instance-ids $INSTANCE_ID \
  --query "Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]" \
  --output table
```

------------------------------------------------------------------------

# Result

-   Instance type changed to **t2.nano**
-   Instance is in **running state**
-   Status checks passed

------------------------------------------------------------------------

# Key Learnings

-   EC2 instance resizing
-   Importance of stopping instance before modification
-   AWS CLI automation
-   Cost optimization
