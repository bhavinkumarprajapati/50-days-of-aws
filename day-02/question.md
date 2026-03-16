# Day 02 -- Create Security Group

## Task

The Nautilus DevOps team is continuing their AWS cloud migration. To
ensure secure communication for application servers, they need to
configure a Security Group in AWS.

For this task, create a **Security Group** under the **default VPC**
with the following requirements.

## Requirements

  Parameter             Value
  --------------------- -----------------------------------------
  Security Group Name   devops-sg
  Description           Security group for Nautilus App Servers
  VPC                   Default VPC
  Region                us-east-1

### Inbound Rules

Add the following inbound rules:

  Type   Protocol   Port Range   Source
  ------ ---------- ------------ -----------
  HTTP   TCP        80           0.0.0.0/0
  SSH    TCP        22           0.0.0.0/0

### Outbound Rules

Leave the **default outbound rule**:

  Type          Protocol   Port Range   Destination
  ------------- ---------- ------------ -------------
  All Traffic   All        All          0.0.0.0/0

The security group should be created inside the **default VPC**.