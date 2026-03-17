# Day 04 -- Enable S3 Versioning

## Objective

Enable versioning on the following S3 bucket:

  Parameter     Value
  ------------- ----------------
  Bucket Name   devops-s3-3446
  Region        us-east-1

------------------------------------------------------------------------

# Concept

**Amazon S3 Versioning** allows you to keep multiple versions of an
object in the same bucket.

## Why Versioning?

-   Protects against accidental deletion
-   Allows rollback to previous versions
-   Helps in data recovery
-   Important for backup and compliance

------------------------------------------------------------------------

# Solution

## Method 1 -- Using AWS Console

### Step 1 -- Login to AWS Console

Select region:

us-east-1

------------------------------------------------------------------------

### Step 2 -- Open S3

Go to S3 service.

------------------------------------------------------------------------

### Step 3 -- Select Bucket

devops-s3-3446

------------------------------------------------------------------------

### Step 4 -- Enable Versioning

1.  Go to Properties\
2.  Find Bucket Versioning\
3.  Click Edit\
4.  Enable versioning\
5.  Save changes

------------------------------------------------------------------------

# Method 2 -- Using AWS CLI

## Enable Versioning

aws s3api put-bucket-versioning\
--bucket devops-s3-3446\
--versioning-configuration Status=Enabled\
--region us-east-1

------------------------------------------------------------------------

## Verify Versioning

aws s3api get-bucket-versioning\
--bucket devops-s3-3446\
--region us-east-1

Expected output:

Status: Enabled

------------------------------------------------------------------------

# Result

Versioning enabled successfully on bucket devops-s3-3446

------------------------------------------------------------------------

# Key Learnings

-   S3 versioning basics\
-   Data protection in AWS\
-   Prevent accidental deletion