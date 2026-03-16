## Day 03 – Solution

### Step 1 – Retrieve AWS Credentials

Run the following command on the AWS client machine:

```bash
showcreds
```

Export the credentials:

```bash
export AWS_ACCESS_KEY_ID=<access_key>
export AWS_SECRET_ACCESS_KEY=<secret_key>
export AWS_DEFAULT_REGION=us-east-1
```

---

### Step 2 – Get Default VPC ID

Retrieve the default VPC ID:

```bash
aws ec2 describe-vpcs \
--filters "Name=isDefault,Values=true" \
--query "Vpcs[0].VpcId" \
--output text \
--region us-east-1
```

Example output:

```
vpc-0abcd1234efgh5678
```

---

### Step 3 – Create Subnet

The default VPC CIDR range is:

```
172.31.0.0/16
```

Most smaller subnet ranges were already allocated in the lab environment, so the available range **`172.31.128.0/20`** was used.

Create the subnet:

```bash
aws ec2 create-subnet \
--vpc-id vpc-0abcd1234efgh5678 \
--cidr-block 172.31.128.0/20 \
--availability-zone us-east-1a \
--region us-east-1
```

---

### Step 4 – Tag the Subnet

Add the required name tag:

```bash
aws ec2 create-tags \
--resources <subnet-id> \
--tags Key=Name,Value=xfusion-subnet \
--region us-east-1
```

Example:

```bash
aws ec2 create-tags \
--resources subnet-0abc123456 \
--tags Key=Name,Value=xfusion-subnet
```

---

### Step 5 – Verify Subnet

Verify that the subnet was created successfully:

```bash
aws ec2 describe-subnets \
--filters "Name=tag:Name,Values=xfusion-subnet" \
--region us-east-1
```

This confirms that the subnet **xfusion-subnet** exists in the default VPC.
