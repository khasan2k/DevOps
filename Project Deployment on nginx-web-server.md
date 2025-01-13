# Nginx Web Server Configuration on Ubuntu 24.04

### 1. Step 1: Clone Project from github in `/var/www/`
```
git clone https://github.com/asifanamkhan/newsbox-nub
```

### 2. Step 2: Installing Nginx
 Update our local package index so that we have access to the most recent package listings. Afterwards, we can install `nginx`.
 	
 ```
sudo apt update
sudo apt install nginx -y
sudo systemctl status nginx
sudo systemctl enable nginx
 ```

### 2. Step 2: Setup Mysql Server.
#### 2.1 Install MySQL
 ```
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
sudo systemctl status mysql
```
#### 2.2 Secure Database
``` 
sudo mysql_secure_installation 
```
     
>> Follow the script prompts below to set up a new root user password, remove anonymous users, disallow remote root login, and remove test databases on your MySQL database server.
 
>>🔷 **VALIDATE PASSWORD** component: `Enter Y` and press Enter to enable password validation on your server.\
🔷 **Password Validation Policy**: `Enter 2` to enable the usage of strong passwords on your server.\
🔷 **New password**: Enter a new strong password to assign the root database user.\
🔷 **Re-enter new password**: Enter your password again to verify the root user password.\
🔷 **Do you wish to continue with the password provided?**: `Enter Y` to apply the new user password.\
🔷 **Remove anonymous users?**: `Enter Y` to revoke MySQL console access to unknown database users.\
🔷 **Disallow root login remotely?**: `Enter Y` to disable remote access to the MySQL root user account on your server.\
🔷 **Remove test database and access to it?**: `Enter Y` to delete the MySQL test databases.\
🔷 **Reload privilege tables now?**: `Enter Y` to refresh the MySQL privilege tables and apply your new configuration changes. 

#### 2.3 Access MySQL
##### Copy Database name from `/var/www/project_name/.env` & update `databese username` and `password` 
```
sudo mysql -u root -p
show databases;
create database `database_name`;
CREATE USER 'tradelicense_user'@'localhost' IDENTIFIED BY 'strong4#Password';
ALTER USER 'tradelicense_user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'strong4#Password';  
GRANT ALL PRIVILEGES ON `database_name`.* TO 'tradelicense_user'@'localhost';
SELECT user, host FROM mysql.user;
flush privileges;
exit
sudo systemctl restart mysql
```

### 5. Steps 5: Enable the file by creating a link from it to the `sites-enabled` directory, which Nginx reads from during startup.

`sudo ln -s /etc/nginx/sites-available/nasir.xyz /etc/nginx/sites-enabled/`

### 6. Steps 6: Test to make sure that there are no syntax errors in any of your Nginx files and restart server.
```
sudo nginx -t
sudo nginx -s reload
sudo systemctl restart nginx
```
### 7. Steps 7: 

- Insert A & CNAME record in your **domain** and Server **Public IP**
- Adjusting your Firewall
- Adjust Other Nginx custom configuration, this is very basic configuration.

All done!!!!!!!!! 🚀💥


# Laravel Project Deployment
 
### 1. Step 1: Install PHP
```
sudo add-apt-repository ppa:ondrej/php -y
sudo apt install php -y
sudo apt-get install php-curl php-xml php-mysql php-mbstring -y
php -v
```
### 2. Step 2: Install Composer
 ```
sudo php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer 	corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
sudo php composer-setup.php
sudo php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
composer
 ```
### 3. Step 3: Synchronize database schema to laravel application

To confirm that the.env file and the Laravel application are in sync, use the command below.
```
sudo composer update --no-scripts
sudo php artisan migrate:refresh --seed
```
 ### Get Permission on /storage folder
```
sudo chown -R www-data:root storage/
```
### 4. Steps 4: Create a server block with the correct directives. Instead of modifying the default configuration file directly.
`sudo vim /etc/nginx/sites-available/hasan.xyz`

#### Add this into hasan.xyz
change root `listen 80` `/var/www/nasir.xyz;` `fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;`
```
#
server {
	 listen 80;
	 listen [::]:80;
	 
	 root /var/www/nasir.xyz;

	 # Add index.php to the list if you are using PHP

	 index index.html index.htm index.php;
	 server_name nasir.xyz www.nasir.xyz;

	 access_log /var/log/nginx/nasir.xyz.access.log;
	 error_log /var/log/nginx/nasir.xyz.error.log;

	 location ~ \.php$ {
		include snippets/fastcgi-php.conf;
		fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
	 }
	 location / {
		#limit_conn concurrent 5;
		#limit_req zone=mylimit burst=5 nodelay;
		try_files $uri $uri/ /index.php?$args;
	 }
	  
	 #Client Side Cashing Configuration
	 location ~* \.(js|jpg|jpeg|gif|png|css|tgz|gz|rar|bz2|doc|pdf|ppt|tar|wav|bmp|rtf|swf|ico|flv|txt|woff|woff2|svg)$ {
	 expires 90d;
	 add_header Pragma public;
	 add_header Cache-Control public;
	 add_header Vary Accept-Encoding;
	 }
	 location @/ {
	 rewrite ^/(.+)$ /index.php?_route_=$1 last;
	 }
	 # Block a single IP address
	 #deny 103.120.35.2;
	 location ~ /\.env {
	 deny all;
	 }
	 location ~ /\. {
	 deny all;
	 }
}
```

# React Node js Project Deployment

### 1. Frontend (React) Deployment Commands
#### Build and Serve the React Application:
Navigate to the React Project Directory:
```
Copy code
cd /path-to-your-react-app
```
#### Install Dependencies (if not already installed):

```bash
npm install
```
#### Build the React Application:

```bash
npm run build
```
#### Set Permissions:

```bash
sudo chown -R www-data:www-data /var/www/react-app
sudo chmod -R 755 /var/www/react-app
```

### 2. Backend (Node.js) Deployment Commands
#### Install and Run the Node.js Application:
Navigate to the Node.js Project Directory:

```bash
cd /path-to-your-node-app
```
#### Install Dependencies:

```bash
npm install
```
#### Test the Application:

```bash
node server.js
```
**Replace server.js with your application's entry point file.**
Ensure it runs correctly and listens on the expected port (e.g., 3000).

### Use PM2 to Manage the Node.js App: Install PM2 globally if not already installed:

```bash
sudo npm install -g pm2
```
#### Start your Node.js application with PM2:

```bash
pm2 start server.js --name node-app
```
#### Save the PM2 process list to ensure it starts on reboot:

```bash
pm2 save
```
#### Set PM2 to start on system boot:

```bash
pm2 startup
```

### 3. Verify Deployment
Frontend: Open a browser and go to:
```
http://your-server-ip/
```
**or**
```
http://localhost/
```
#### Backend API: Test the API endpoint in the browser or via curl:

```bash
curl http://your-server-ip/api/some-endpoint
```
### 5. Debugging
#### Check Nginx logs for errors:
```bash
sudo tail -f /var/log/nginx/error.log
```
#### Check PM2 logs for Node.js errors:
```bash
pm2 logs node-app
```

# Important but not on the list
### Unlink  
sudo unlink /etc/nginx/sites-enabled/default
 
