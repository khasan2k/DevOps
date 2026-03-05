## Day 23: Data Migration Between S3 Buckets Using AWS CLI
As part of a data migration project, the team lead has tasked the team with migrating data from an existing S3 bucket to a new S3 bucket. The existing bucket contains a substantial amount of data that must be accurately transferred to the new bucket. The team is responsible for creating the new S3 bucket and ensuring that all data from the existing bucket is copied or synced to the new bucket completely and accurately. It is imperative to perform thorough verification steps to confirm that all data has been successfully transferred to the new bucket without any loss or corruption.

- As a member of the Nautilus DevOps Team, your task is to perform the following:
- Create a New Private S3 Bucket: Name the bucket datacenter-sync-14788.
- Data Migration: Migrate the entire data from the existing datacenter-s3-12806 bucket to the new datacenter-sync-14788 bucket.
- Ensure Data Consistency: Ensure that both buckets have the same data.
- Use AWS CLI: Use the AWS CLI to perform the creation and data migration tasks.


### 1️⃣ Make Sure AWS CLI Is Installed
Check:
```
aws --version
```
**If not installed (Amazon Linux / RHEL / CentOS):**
```
sudo dnf install awscli -y
```
### 2️⃣ Configure AWS CLI (If Not Already)
```
aws configure
```
**Enter:**   
AWS Access Key ID:   
AWS Secret Access Key:   
Default region name: us-east-1   (or your region)   
Default output format: json   

### 3️⃣ Create a Private S3 Bucket   
⚠ Bucket names must be globally unique.   
Example:
```
aws s3api create-bucket \
  --bucket kamrul-private-bucket-2026 \
  --region us-east-1
```

### 4️⃣ Ensure Bucket Is Private (Important)

By default, S3 buckets are private.   
But you can explicitly block public access:
```
aws s3api put-public-access-block \
  --bucket kamrul-private-bucket-2026 \
  --public-access-block-configuration \
  BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
```
### 5️⃣ Verify Bucket
```
aws s3api get-bucket-acl --bucket kamrul-private-bucket-2026
```
or
```
aws s3 ls
```
## Data Migration:

### ✅ Step 1: Sync Entire Data (Recommended Method)   
Run:
```
aws s3 sync s3://xfusion-s3-21295 s3://xfusion-sync-18131
```
### ✅ Step 3: Verify Migration
Check new bucket:
```
aws s3 ls s3://xfusion-sync-18131 --recursive
```
