## Day 16: Install and Configure Nginx as an LBR
### Problem:
Day by day traffic is increasing on one of the websites managed by the Nautilus production support team. Therefore, the team has observed a degradation in website performance. Following discussions about this issue, the team has decided to deploy this application on a high availability stack i.e on Nautilus infra in Stratos DC. They started the migration last month and it is almost done, as only the LBR server configuration is pending. Configure LBR server as per the information given below:

**Tasks:**   
1 - Install nginx on LBR (load balancer) server.   
2 - Configure load-balancing with the an http context making use of all App Servers. Ensure that you update only the main Nginx configuration file located at /etc/nginx/nginx.conf.   
3 - Make sure you do not update the apache port that is already defined in the apache configuration on all app servers, also make sure apache service is up and running on all app servers.   

**এক লাইনে সংক্ষেপে**    
একটি single-server–ভিত্তিক ওয়েবসাইটে অতিরিক্ত ট্রাফিকের কারণে পারফরম্যান্স সমস্যা দেখা দিয়েছে, তাই Nginx ব্যবহার করে Load Balancer কনফিগার করে High Availability নিশ্চিত করতে হবে।   

**🎯 কী করা দরকার?**
- LBR সার্ভারে Nginx ইনস্টল করতে হবে   
- Nginx ব্যবহার করে HTTP-based load balancing কনফিগার করতে হবে   
- Backend হিসেবে সব App Server (Apache) ব্যবহার করতে হবে   
- Apache-এর বিদ্যমান port পরিবর্তন করা যাবে না   
- Apache সার্ভিস সব App Server–এ চালু থাকতে হবে   
- কনফিগারেশন করতে হবে শুধু /etc/nginx/nginx.conf ফাইল ব্যবহার করে
  
### Solution: 

#### Step 1: Install Nginx on LBR Server
```
sshpass -p 'Mischi3f' ssh -o StrictHostKeyChecking=no  loki@172.16.238.14
sudo su
dnf install nginx -y
systemctl restart nginx
systemctl status nginx   
```
#### Step 2: which port Apache is using on stapp01 - its 5001. Double check this is the same for stapp02 and stapp03.
```
sshpass -p  'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10   
sudo su
```
> netstat -tulpn | grep httpd - netstat isnt found so use ss
```
ss -tulpn | grep httpd
tcp   LISTEN 0      511          0.0.0.0:5001       0.0.0.0:*
```
#### Step 2: Return back to stlb001 and make the nessecary changes in the nginx.conf file.
```
vi /etc/nginx/nginx.conf

#Within the http section add the following:
upstream appservers {
        server stapp01:5001;
        server stapp02:5001;
        server stapp03:5001;
    }

#Within the server section add the following:
location / {
    proxy_pass http://appservers;  <-- This must match the upstrean name
}

```
```
nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful

systemctl restart nginx
```

## Day 17: Install and Configure PostgreSQL   
### Step 1: PostgreSQL server-এ login   
```
sudo -i -u postgres
psql
```
### 🔹Step 2: Create new User with password   
```
CREATE USER appuser WITH PASSWORD 'apppass';
```
### 🔹 Step 3: Create new Database   
```
CREATE DATABASE appdb;
```
### 🔹 Step 4: Set user as a owner of Database   
```
ALTER DATABASE appdb OWNER TO appuser;
```
### 🔹 Step 5: Ensure the Permission   
```
GRANT ALL PRIVILEGES ON DATABASE appdb TO appuser;
```
### 🔹 Step 6: Verify (Check)
**See User list**   
```
\du
```
**See Database list**   
```
\l
```
### 🔹 Step 7: User দিয়ে Login Test   
**Now test:**
```
psql -U appuser -d appdb -h localhost

```
Password চাইবে → apppass   

## Day 19: Install and Configure Web Application

xFusionCorp Industries is planning to host two static websites on their infra in Stratos Datacenter. The development of these websites is still in-progress, but we want to get the servers ready. Please perform the following steps to accomplish the task: 
- a. Install httpd package and dependencies on app server 3.
- b. Apache should serve on port 3003.
- c. There are two website's backups /home/thor/blog and /home/thor/apps on jump_host. Set them up on Apache in a way that blog should work on the link http://localhost:3003/blog/ and apps should work on link http://localhost:3003/apps/ on the mentioned app server.
- d. Once configured you should be able to access the website using curl command on the respective app server, i.e curl http://localhost:3003/blog/ and curl http://localhost:3003/apps/

### Step 1: Access App Server 2
```
ssh steve@stapp02
```
### Step 2: Install Apache (httpd)
```
sudo yum install -y httpd
```
Enable and start the service:
```
sudo systemctl enable httpd
sudo systemctl start httpd
```
### Step 3. Change Apache to Listen on Port 5004
Edit the main Apache config file:
```
sudo vi /etc/httpd/conf/httpd.conf
```
Look for the line:
```
Listen 80
```
And change it to:
```
Listen 5004
```
### Step 4. Copy Website Backups from Jump Host to App Server 2

On jump_host, copy the two folders to stapp02:
```
scp -r /home/thor/official steve@stapp02:/tmp/
scp -r /home/thor/apps steve@stapp02:/tmp/
```
Now SSH back to App Server 2:
```
ssh steve@stapp02
```
Then move the two folders into Apache’s web root:
```
sudo mv /tmp/official /var/www/html/
sudo mv /tmp/apps /var/www/html/
```
### Step 5. Configure Apache for Virtual Directories
Open the Apache config again:
```
sudo vi /etc/httpd/conf/httpd.conf
At the bottom of the document add this line:

<VirtualHost *:5004>
    DocumentRoot /var/www/html
    <Directory "/var/www/html">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
Since the folders /var/www/html/official and /var/www/html/apps already exist, Apache will automatically serve them as subdirectories:
```
http://localhost:5004/official/
http://localhost:5004/apps/
```
### Step 7. Test with Curl
```
curl http://localhost:5004/official/
curl http://localhost:5004/apps/
```
You should be able to see the index content of each site
























