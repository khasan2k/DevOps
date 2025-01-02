# How To Deploy Laravel Project using Nginx Server on Ubuntu 24.04

### 1. Step 1: Installing Nginx
 Update our local package index so that we have access to the most recent package listings. Afterwards, we can install `nginx`.
 	
 ```
	sudo apt update
	sudo apt install nginx -y
	sudo systemctl status nginx
	sudo systemctl enable nginx
 ```
### 2. Step 2: Clone Project from github in `/var/www/`
```
	git clone https://github.com/asifanamkhan/newsbox-nub
```
### 3. Step 3: Install PHP and Composer.
```
	sudo add-apt-repository ppa:ondrej/php -y
	sudo apt -y install php -y
	sudo apt-get install php-curl php-xml php-mysql php-mbstring -y
 	php -v
```
#### Composer
 ```
  	php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
	php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer 	corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
	php composer-setup.php
	php -r "unlink('composer-setup.php');"
	sudo mv composer.phar /usr/local/bin/composer
	composer
 ```

### 4. Step 4: Setup Mysql Server.
#### 4.1 Install MySQL
 ```
  	sudo apt install mysql-server -y
   	sudo systemctl start mysql
 	sudo systemctl enable mysql
	sudo systemctl status mysql
```
#### 4.2 Secure Database
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

#### 4.3 Access MySQL
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
### 5. Step 5: Synchronize database schema to laravel application

To confirm that the.env file and the Laravel application are in sync, use the command below.
```
  	php artisan migrate:refresh --seed
   	composer update --no-scripts
```

### 6. Steps 6: Create a server block with the correct directives. Instead of modifying the default configuration file directly.
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
 ### Get Permission on /storage folder
```
    	sudo chown -R www-data:root storage/
```
### 7. Steps 7: Enable the file by creating a link from it to the `sites-enabled` directory, which Nginx reads from during startup.

	`sudo ln -s /etc/nginx/sites-available/nasir.xyz /etc/nginx/sites-enabled/`

### 8. Steps 8: Test to make sure that there are no syntax errors in any of your Nginx files and restart server.
```
	sudo nginx -t
	sudo nginx -s reload
	sudo systemctl restart nginx
```
### 9. Steps 9: 

	- Insert A & CNAME record in your **domain** and Server **Public IP**
	- Adjusting your Firewall
	- Adjust Other Nginx custom configuration, this is very basic configuration.

	All done!!!!!!!!! 🚀💥

 # Important but not on the list
 ### Uninstall Apache2
 ```
  	sudo service apache2 stop
   	sudo apt-get purge apache2 apache2-utils apache2.2-bin apache2-common
	sudo apt-get purge apache2
 	sudo apt remove apache2*
 	sudo rm -Rf /etc/apache2 /usr/lib/apache2 /usr/include/apache2
  	sudo apt-get autoremove --purge
 ```
 
