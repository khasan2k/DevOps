
# Traditinal Deployment

## Step 1: Install MySQL 8 Database and Database and User for backend app

```bash
CREATE DATABASE studentdb;
CREATE USER 'student_user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Nasir#4321';
GRANT ALL PRIVILEGES ON studentdb.* TO 'student_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## Step 2: Install OpenJDK 21 or Higher

```
sudo apt install default-jdk
java --version
```
## Step 3: Install Maven

Download the Maven Binaries [from here](https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz)

```bash
sudo apt install maven -y
mvn -v
```
## Step 4: Build the Backend Application

God to backend project directory and run `mvn clean install -DskipTests`  
`mvn clean package`   
after making .jar file run your jar file to `sudo java -jar student-0.0.1-SNAPSHOT.jar`



## Step 5: Create Systemd Service for backend app

Create Systemd Service you can [follow](https://github.com/nasirnjs/LinuxOpsHub/blob/main/create_systemd_service.md)

**Note** Make sure your JDK and Maven path `which java` `which mvn` for systemd service.

`sudo vim /etc/systemd/system/lendingapp.service`

```bash
[Unit]
Description=Lending Application

[Service]
Type=simple
Restart=always
User=root
Group=www-data

# Define the log files
StandardOutput=append:/var/log/lendingapp.service.log
StandardError=append:/var/log/lendingapp.service.error.log

# Project root path
WorkingDirectory=/var/www/html/

# Command to execute the JAR file
ExecStart=/usr/local/jdk-18/bin/java -jar /var/www/html/student-0.0.1-SNAPSHOT.jar

[Install]
WantedBy=multi-user.target
```
`sudo systemctl daemon-reload`
`sudo systemctl enable lendingapp.service`
`sudo systemctl start lendingapp.service`
`sudo systemctl status lendingapp.service`


## Step 6: Run and Build Frontend

Install Node Version you can follow this [stesps](https://github.com/nasirnjs/LinuxOpsHub/blob/main/install-node-via-nvode-versionmanager.md#installing-node-using-the-node-version-manager) and Install Angular CLI globally.

Installing Node Using the Node Version Manager from [Here](https://github.com/nasirnjs/LinuxOpsHub/blob/main/install-node-via-nvode-versionmanager.md)

`sudo npm install -g @angular/cli`  
`sudo apt install npm -y`  
`sudo npm run build`  
`sudo npm start`  
`node --version`  

## Step 7: Configure Your Nginx Web Server
```
cp -r /etc/nginx/sites-available/default /etc/nginx/sites-available/docker-student.io 
sudo vim /etc/nginx/sites-available/docker-student.io
sudo ln -s /etc/nginx/sites-available/docker-student.io /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

## Step 8: Update IP address for backend
`cd /var/www/docker-student-sync/student-fe/src/app/service$ sudo vim student.service.ts`
`const BASIC_URL = ["http://192.168.119.129:8080/"]`   
