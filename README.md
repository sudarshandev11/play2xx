Here's a detailed **README.md** file for Ubuntu 20.04 setup that is formatted for ease of copying and use.

---

# Ubuntu 20.04 Setup Guide

## Global Install

```bash
# Update and install necessary packages
sudo apt-get update
sudo apt-get -y install software-properties-common curl unzip zip

# Add repositories for PHP and phpMyAdmin
sudo add-apt-repository ppa:phpmyadmin/ppa -y
sudo add-apt-repository ppa:ondrej/php -y

# Update packages and upgrade
sudo apt-get update
sudo apt-get --with-new-pkgs upgrade -y

# Install PHP 7.4 and required extensions
sudo apt-get install -y php7.4 php7.4-bcmath php7.4-ctype php7.4-fileinfo php7.4-json php7.4-mbstring php7.4-pdo php7.4-xml php7.4-tokenizer

# Install curl and PHP curl dependencies
sudo apt-get install curl libcurl3 libcurl3-dev php7.4-curl -y

# Install other required packages
sudo apt-get install php7.4-gd -y
sudo apt-get install freetype* -y
sudo apt-get install -y composer apache2 mysql-server

# Secure MySQL installation
sudo mysql_secure_installation
```

## MySQL Setup

```bash
# Access MySQL as root
sudo mysql -u root

# Set root password and use MySQL native authentication
USE mysql;
UPDATE user SET authentication_string=PASSWORD("bX3rT3cU") WHERE User='root';
UPDATE user SET plugin="mysql_native_password" WHERE User='root';
FLUSH PRIVILEGES;
quit
```

## phpMyAdmin Setup

```bash
# Install phpMyAdmin (Select Apache during installation)
sudo apt-get install -y phpmyadmin
```

## Node.js and pm2 Setup

```bash
# Install Node.js 10.x and pm2
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
sudo npm i -g pm2
```

## Upload Source Code and Setup Permissions

```bash
# Navigate to /var/www/html and upload your source code (assuming via unzip)
cd /var/www/html
sudo unzip archive_name.zip

# Set proper permissions for storage and bootstrap directories
sudo chmod -R 777 /var/www/html/storage
sudo chmod -R 777 /var/www/html/bootstrap

# Install Composer globally
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

## SSL Installation

```bash
# Install Certbot for SSL and configure it with Apache
sudo apt install certbot python3-certbot-apache -y
sudo systemctl reload apache2
sudo certbot --apache

# Follow the steps:
# - First stage: Enter your email
# - Second stage: Press A
# - Third stage: Press Y
# - Fourth stage: Enter your domain name without slashes and protocols, e.g., urdomain.com

# Check the status of certbot timer
sudo systemctl status certbot.timer
```

## Apache Configuration

```bash
# Edit the Apache configuration files to point to your public directory

# Open and edit the following files:
# - /etc/apache2/sites-available/000-default.conf
# - /etc/apache2/sites-available/000-default-le-ssl.conf

# Replace the line:
DocumentRoot /var/www/html

# With:
DocumentRoot /var/www/html/public
<Directory /var/www/html/public>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>

# Enable Apache rewrite module and restart the service
sudo a2enmod rewrite
sudo service apache2 restart
```

## Node & npm Configuration

```bash
# Navigate to the project directory
cd /var/www/html

# Install npm and required packages
sudo apt-get install npm -y
sudo npm install --save -g pm2 express http https xss-filters crypto mathjs socket.io fs

# Modify domain in app.js file
# Change the following line:
domain = __LOCALHOST ? 'http://localhost' : 'http://your_domen.com';

# Start the Node.js app using pm2
pm2 start app.js
```

## Startup Project

```bash
# Update environment variables in the .env file
nano /var/www/html/.env

# Set the following values:
APP_DEBUG=false
DB_DATABASE=name_of_your_database
DB_USERNAME=your_database_username
DB_PASSWORD=your_database_password

# Modify port in app.js (secure port)
nano /var/www/html/public/js/app.js

# Change the port from 49299 to 8443 (or the port you are using):
# Example:
const port = 8443;

# Save and exit

# Laravel Artisan Commands (if using Laravel)
php artisan config:cache
php artisan optimize
php artisan view:cache
```

## Final Setup

```bash
# After completing all steps, visit your site and set the default settings in the admin page settings.
# Your setup should now be complete.
```

---

With this **README.md**, you can follow along to set up the server on Ubuntu 20.04, ensuring that all commands are properly formatted for easy copy-paste usage.
