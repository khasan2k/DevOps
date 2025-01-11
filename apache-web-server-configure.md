# How To Install the Apache Web Server on Ubuntu 22.04
***
### 1. **Step 1: Installing Apache**
   **Update our local package index so that we have access to the most recent package listings. Afterwards, we can install Apache.**

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl status apache2
sudo systemctl enable apache2
```
  **Verify Apache Installation.**
```
http://server_ip_address
   ```
### 2. Step 2: Clone Project from github in `/var/www/`
```
git clone https://github.com/asifanamkhan/newsbox-nub
```
### 3. Step 3: Install PHP and Composer.
```
sudo add-apt-repository ppa:ondrej/php -y
sudo apt install php -y
sudo apt-get install php-curl php-xml php-mysql php-mbstring -y
php -v
```
#### Composer
```
sudo apt install composer -y
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
#### Import database manually
```
use `database_name`;
source `location/database_file`;
```
### 5. Step 5: Synchronize database schema to laravel application

**To confirm that the.env file and the Laravel application are in sync, use the command below.**
```
sudo composer update --no-scripts
sudo php artisan migrate:refresh --seed
```
 ### Get Permission on /storage folder
```
 sudo chown -R www-data:root storage/
```

### 6. **Step 6: Create a Server Block Virtual Hosts**
```
sudo vim /etc/apache2/sites-available/cloudopsschool.com.conf

```

```
    <VirtualHost *:80>
        ServerAdmin webmaster@cloudopsschool.com
        ServerName server_ip
        ServerAlias www.cloudopsschool.com
        DocumentRoot /var/www/cloudopsschool.com/

        # Customizing ErrorLog and CustomLog
        ErrorLog /var/log/apache2/nasir_error.log
        CustomLog /var/log/apache2/nasir_access.log combined
    </VirtualHost>
```

### 7. `Steps 7: Enable the File and Reload Apache.` Enable the file by creating a link from it to the sites-enabled directory, which Apache reads from during startup.
```bash
sudo a2ensite cloudopsschool.com.conf
sudo systemctl reload apache2
```

### 7. `Steps 7: Test and Restart Apache.` Test to make sure that there are no syntax errors in any of your Apache files and restart the server.
```bash
sudo apache2ctl configtest
sudo apachectl -k graceful
sudo systemctl restart apache2
```

### 8. `Steps 7: Additional Configuration`

- Insert A & CNAME records in your domain and Server Public IP
- Adjusting your Firewall
- Other Apache2 custom configurations; this is a very basic configuration.
  
    **All Done!!!!!!!!! 🚀💥**

# Important but not on the list
### Error & Solutions:

### Problem: 
**AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message**

### Solution:  
```
sudo vim /etc/apache2/apache2.conf
```
**Add a line containing ServerName 127.0.0.1 to the end of the file:**
#Include the virtual host configurations:
IncludeOptional sites-enabled/*.conf

```
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
ServerName 127.0.0.1
```


 


