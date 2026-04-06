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


## Day 30: Enable Internet Access for Private EC2 using NAT Instance
### Problem:
The Nautilus DevOps team is tasked with enabling internet access for an EC2 instance running in a private subnet. This instance should be able to upload a test file to a public S3 bucket once it can access the internet. To achieve this, the team must set up a NAT Gateway in a public subnet within the same VPC.

1) A VPC named `xfusion-priv-vpc` and a private subnet `xfusion-priv-subnet` have already been created.
2) An EC2 instance named `xfusion-priv-ec2` is already running in the private subnet.
3) The EC2 instance is configured with a cron job that uploads a test file to a `bucket xfusion-nat-11088` once internet is accessible.

Your task is to:

- Create a new public subnet named `devops-pub-subnet` in the existing VPC.
- Launch a **NAT Instance** in the public subnet using an `Amazon Linux 2` AMI and name it `devops-nat-instance`. Configure this instance to act as a **NAT instance**. Make sure to use a custom security group for this instance.

After the configuration, verify that the test file `devops-test.txt` appears in the S3 bucket `devops-nat-18335`. This indicates successful internet access from the private EC2 instance via the NAT Instance.

Once complete, verify that the EC2 instance can reach the internet by confirming the presence of the test file in the S3 `bucket xfusion-nat-11088`. After completing all the configuration, please wait a few minutes for the test file to appear in the bucket, as it may take 2–3 minutes.


### Task 1: Create a public subnet

1. **From VPC dashboard, select subnet and click `Create`.**
2. **Select `xfusion-priv-vpc`.**

   <img width="1318" height="222" alt="image" src="https://github.com/user-attachments/assets/009acfa2-4f4e-42cb-89fe-e41384d337ca" />

3. **Under `Subnet settings` section, enter subnet name and select valid CIDR block.**

   <img width="1318" height="462" alt="image" src="https://github.com/user-attachments/assets/fbe3e19c-85f9-4ef2-8a94-7c69a51e8cb7" />


### Task 2: Create an Internet Gateway and attach it to the VPC

1. **From VPC dashboard, create an internet gateway with any relavant name.**

   <img width="1312" height="436" alt="image" src="https://github.com/user-attachments/assets/f8cc4b78-7069-44ce-9ed1-498a5510a31f" />

2. **Preview all internet gaetways.**

   <img width="1149" height="171" alt="image" src="https://github.com/user-attachments/assets/aad3452e-466a-4662-96c5-10fc5a23ef6a" />

   **You can see that newly created internet gateway is detached.**

3. **To attach the internet gateway to `xfusion-priv-vpc`, select `xfusion-ig` and choose `Action` > `Attach`. and select `xfusion-priv-vpc`**

   <img width="1298" height="262" alt="image" src="https://github.com/user-attachments/assets/b3878b30-65c8-4826-a051-819f9af50d48" />


### Task 3: Create a route table

1. **Create a route table in `xfusion-priv-vpc`**

   <img width="1328" height="440" alt="image" src="https://github.com/user-attachments/assets/3c7e0bd7-a114-4ea2-aefa-dcae1f2810fa" />

2. **After creating route table, edit its route to internet gateway `xfusion-ig` you just created.**

   <img width="1366" height="356" alt="image" src="https://github.com/user-attachments/assets/c0433b06-e92d-4aed-99de-e57bf4ee22d5" />

3. **Next, change `xfusion-pub-subnet` route table association to `xfusion-pub-rt`.**

   <img width="1365" height="428" alt="image" src="https://github.com/user-attachments/assets/33bed471-690b-4241-be49-e07345b92997" />

4. **Now select `xfusion-priv-vpc` and see its resource map.**

   <img width="935" height="182" alt="image" src="https://github.com/user-attachments/assets/53cac65e-8d12-4808-9d38-19e776e0fbcc" />

   **You can notice that `xfusion-pub-subnet` is showing as public subnet now.**

### Task 4: Allocate an Elastic IP (Optional)

1. **From VPC dashboard menu, select `Elastic IPs` and click `Allocate Elastic IP Address`.**

   <img width="1125" height="200" alt="image" src="https://github.com/user-attachments/assets/e4b8364d-27a9-480a-b21b-cef1b3d48d7c" />

>[!Note]
> **Elastic IP can be allocated during NAT Gateway creation too. (PREFFERED)**

### Task 5: Create a NAT Gateway

1. **From VPC dashboard menu, select `NAT Gateways` and click `Create NAT gateways`.**

   <img width="1125" height="119" alt="image" src="https://github.com/user-attachments/assets/2805aa25-1986-41d2-8d42-04936c28e49b" />

2. **Enter gateway name and select `Availability mode` as `Zonal` and `xfusion-pub-subnet`.**

   <img width="1294" height="471" alt="image" src="https://github.com/user-attachments/assets/ca4f4c36-697a-4722-a38e-4f941907eb49" />

3. **Wait until `Status` = `Available`.**
   
   <img width="1108" height="340" alt="image" src="https://github.com/user-attachments/assets/c7972fe2-38b6-4a19-870b-04c3d452dd79" />


### Task 6: Update the private route table to route 0.0.0.0/0 traffic via the NAT Gateway.

1. **Go to **VPC** > **Route Tables** and identify the route table associated with: `xfusion-priv-subnet`**

   <img width="1173" height="196" alt="image" src="https://github.com/user-attachments/assets/76383851-16be-4441-b53e-79c6c94d7083" />

2. **Also associate private route table with private subnet by selecting the private route table's `Edit subnet associate` tab.**

   <img width="1149" height="194" alt="image" src="https://github.com/user-attachments/assets/ff20cf3a-673b-4c1f-b8b7-4223c5f32275" />
  
3. Go to **Routes** > **Edit routes and update the private route table to route `0.0.0.0/0` traffic to the NAT Gateway.**

   <img width="1332" height="241" alt="image" src="https://github.com/user-attachments/assets/dae7dbf8-091e-4b3c-8125-0283ac72414a" />

### Task 7: Verify instance can access S3 Bucket

1. **Go to S3 Bucket and select the existing bucket under `General purpose bucket` section.**

   <img width="1018" height="184" alt="image" src="https://github.com/user-attachments/assets/e653cc35-b811-4577-8805-cf5de567947c" />

2. **Click on the bucket and you should see the test file uploaded by the EC2 instance.**

    <img width="1063" height="446" alt="image" src="https://github.com/user-attachments/assets/6dc28e25-9294-4aa0-9de7-6e7c43f0147c" />

### Task 8: Launch EC2 Instance in Public VPC

 Now create `devops-nat-instance` under `devops-pub-subnet` using an `Amazon Linux 2` AMI and configure this instance to act as a **NAT instance**.

   <img width="851" height="354" alt="image" src="https://github.com/user-attachments/assets/f8166351-daec-4aac-b6d8-8a45a1ebb305" />

7. Make sure to choose `devops-priv-vpc` and subnet `devops-pub-subnet`. Also enable `Auto-assign public IP`

   <img width="859" height="426" alt="image" src="https://github.com/user-attachments/assets/f43bdb4c-438b-41e3-9198-3cd6b25706d3" />

8. Change **inbound** security group rules for instance as follows:
   - Choose **Add rule**. Choose **HTTP** for Type and enter the IP address range of **your private subnet** for **Source**.
   - Choose **Add rule**. Choose **HTTPS** for Type and enter the IP address range of **your private subnet** for **Source**.
   - Choose **Add rule**. Choose **SSH** for Type and enter the IP address range of **your network** for **Source**.

     <img width="1315" height="369" alt="image" src="https://github.com/user-attachments/assets/94b5d6bf-c7cd-475f-bc62-a28b2a59547e" />

10. Change **outbound** security group rules for instance as follows:

    - Choose **Add rule**. Choose **HTTP** for Type and enter **0.0.0.0/0** for **Destination**.
    - Choose **Add rule**. Choose **HTTPS** for Type and enter **0.0.0.0/0** for **Destination**.

      <img width="1315" height="370" alt="image" src="https://github.com/user-attachments/assets/5a5ee93f-28a3-43c9-a0a0-9c0af5f78a0d" />

12. **Disable Source/Dest Check**: (Critical) Select `Instance > Actions > Networking > Change source/destination check > Set to Stop/Disable`.

    <img width="600" height="343" alt="image" src="https://github.com/user-attachments/assets/32b9fc07-85ad-4976-ba0b-ca89664bb4db" />


## To create a NAT AMI for Amazon Linux

1. Configure OS-Level NAT (IP Tables)

   SSH into the NAT instance and run the following:

   ```bash
   sudo yum install iptables-services -y
   sudo systemctl enable iptables
   sudo systemctl start iptables
   ```
   Enable IP forwarding in the kernel
   ```bash
   echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
   sudo sysctl -p
   ```
   Configure IP tables to route traffic from the private subnet
   ```bash
   sudo iptables -t nat -A POSTROUTING -o eth0 -s 10.1.1.0/24 -j MASQUERADE
   sudo service iptables save
   ```

  2. Update Private Subnet Routing (The "Private" Side)

   Add Route: Destination: `0.0.0.0/0` -> Target: Instance -> Select your `devops-nat-instance`.
   
   <img width="1366" height="344" alt="image" src="https://github.com/user-attachments/assets/ddddf4a7-2378-4cff-ae8c-98ef61ae3ade" />
  




