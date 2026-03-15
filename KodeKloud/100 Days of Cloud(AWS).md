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

## Task Day 29: Establishing Secure Communication Between Public and Private VPCs via VPC Peering

The Nautilus DevOps team has been tasked with demonstrating the use of VPC Peering to enable communication between two VPCs. One VPC will be a private VPC that contains a private EC2 instance, while the other will be the default public VPC containing a publicly accessible EC2 instance. 
1) There is already an existing EC2 instance in the public vpc/subnet: Name: nautilus-public-ec2
2)  2) There is already an existing Private VPC: Name: nautilus-private-vpc CIDR: 10.1.0.0/16
3) There is already an existing Subnet in nautilus-private-vpc: Name: nautilus-private-subnet CIDR: 10.1.1.0/24
4) There is already an existing EC2 instance in the private subnet: Name: nautilus-private-ec2
5) Create a Peering Connection between the Default VPC and the Private VPC: VPC Peering Connection Name: nautilus-vpc-peering
6) Configure Route Tables to enable communication between the two VPCs. Ensure the private EC2 instance is accessible from the public EC2 instance.
7) Test the Connection: Add /root/.ssh/id_rsa.pub public key to the public EC2 instance's ec2-user's authorized_keys to make sure we are able to ssh into this instance from AWS client host. You may also need to update the security group of the private EC2 instance to allow ICMP traffic from the public/default VPC CIDR. This will enable you to ping the private instance from the public instance.   
    SSH into the public EC2 instance and ensure that you can ping the private EC2 instance.

### Solution: 
এই lab-এ আপনাকে Amazon Virtual Private Cloud ব্যবহার করে VPC Peering configure করতে হবে যাতে public VPC থেকে private VPC-র EC2 instance-এ ping করা যায়।   
আমি step-by-step দিচ্ছি 👇

#### 1️⃣ VPC Peering Connection তৈরি করুন

`AWS Console → VPC → Peering Connections → Create Peering Connection`
#### Details
**Name:** nautilus-vpc-peering   
**Requester VPC:** Default VPC   
**Accepter VPC:** nautilus-private-vpc   
তারপর Actions → Accept request করুন।

#### 2️⃣ Route Table Configure করুন

দুই VPC-তেই route add করতে হবে।

#### Public VPC Route Table

`VPC → Route Tables → default VPC route table edit করুন।`

#### Add route:

**Destination:** 10.1.0.0/16   
**Target:** nautilus-vpc-peering

#### Private VPC Route Table
nautilus-private-vpc এর route table edit করুন।
#### Add route:
**Destination:** <Default VPC CIDR>   
**Target:** nautilus-vpc-peering

##### Default VPC CIDR সাধারণত:
`172.31.0.0/16`

#### 3️⃣ Security Group Update করুন

`Private EC2 instance (nautilus-private-ec2) এর security group edit করুন।`

#### Add inbound rule:

**Type:** ICMP - IPv4   
**Source:** Default VPC CIDR (example 172.31.0.0/16)   
এটা ping allow করবে।

#### 4️⃣ Public EC2 তে SSH Key Add করুন

AWS client host থেকে public key copy করুন।
```
cat /root/.ssh/id_rsa.pub
```

Public EC2 এ login করুন:
```
ssh ec2-user@<public-ec2-ip>
```

তারপর:
```
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```
Public key paste করুন।   

#### Permission fix করুন:
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
#### 5️⃣ Public EC2 থেকে Private EC2 Ping Test

#### Public EC2 এ login করুন:
```
ssh ec2-user@<public-ec2-ip>
```
#### Private EC2 IP check করুন:
```
ping <private-ec2-private-ip>
```
যদি সব ঠিক থাকে, ping reply আসবে।


