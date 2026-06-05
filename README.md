# ICT171-ASSIGNMENT3-VS

**Name:** Vaibhav Saini

**Student ID:** 35819283

**Public IP Address:** http://20.5.17.115/

**Domain Name:** https://vaibhavblogs.com/


## Project Overview

The objective of this project was to design, deploy, and manage a cloud-hosted server environment using Microsoft Azure. An Ubuntu Linux virtual machine was configured to host a WordPress blogging platform using Nginx, PHP, and MariaDB.

A custom domain name (vaibhavblogs.com) was integrated using DNS records, and HTTPS encryption was implemented using Let's Encrypt SSL/TLS certificates. In addition to the website deployment, a custom Bash script was developed to automate WordPress backups and generate a web-based server status report.

The project demonstrates cloud server deployment, web server administration, DNS management, SSL/TLS implementation, WordPress hosting, Linux system administration, and Bash scripting.


## Security Maintenance and Vulnerability Patching

A new CVE vulnerability affecting Nginx servers on Ubuntu systems was publicly disclosed. Since the cloud server is publicly accessible on the Internet, security maintenance and vulnerability patching are important responsibilities for system administrators.

To reduce potential security risks, the server packages were updated using the Ubuntu package manager. The Nginx service was then restarted to ensure that the updated version was running correctly.

The following commands were used during the maintenance process:

```bash
sudo apt update && sudo apt upgrade
sudo systemctl restart nginx
```

After the update process was completed, the website was tested through the public IP address to confirm that the Nginx web server was operating correctly and that the website remained publicly accessible.
## DNS Configuration and Domain Integration

A custom domain name was purchased through GoDaddy to improve the accessibility and professionalism of the cloud-hosted blogging platform.

The domain was manually connected to the Microsoft Azure virtual machine using DNS records. An A record was created to point the domain to the Azure public IP address, while a CNAME record was configured for the www subdomain.

### DNS Records Configured

#### A Record
- Type: A
- Host/Name: @
- Points to/Value: 20.5.17.115
- TTL- 1/2 HOUR

#### CNAME Record
- Type: CNAME
- Host/Name: www
- Points to/Value: vaibhavblogs.com
- TTL: 1/2 HOUR
## SSL/TLS HTTPS Configuration

SSL/TLS encryption was implemented using Let's Encrypt and Certbot to improve the security and accessibility of the cloud-hosted web server.

The Certbot package for Nginx was installed on the Ubuntu virtual machine, and a trusted SSL certificate was generated for both the primary domain and the www subdomain.

### Commands Used

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d vaibhavblogs.com -d www.vaibhavblogs.com
```

After doing these steps the encrption was done
https://vaibhavblogs.com

## Microsoft Azure Virtual Machine Deployment

A Linux virtual machine was deployed using Microsoft Azure Infrastructure as a Service (IaaS). The virtual machine serves as the hosting environment for this cloud server project.

### Virtual Machine Configuration

* **Cloud Provider:** Microsoft Azure
* **Operating System:** Ubuntu Server
* **Public IP Address:** 20.5.17.115

The virtual machine was created through the Azure Portal and assigned a public IP address to allow remote administration and public access to hosted services.

### Verification

The successful deployment of the virtual machine was verified by connecting to the server using SSH from a local Windows computer.

---

## Ubuntu Server Configuration

Ubuntu Server was selected as the operating system due to its stability, security, and widespread use in cloud environments.

Remote administration was performed through SSH, allowing secure command-line access to the server.

### SSH Connection Command

```bash
ssh -i "vsaini14711_key (1).pem" vaibhavsaini@20.5.17.115
```

### Verification

Successful login to the Ubuntu server confirmed that network connectivity and remote administration were functioning correctly.

---

## Nginx Web Server Installation

Nginx was selected as the web server software due to its performance, reliability, and popularity in modern web hosting environments.

### Commands Used

```bash
sudo apt update

sudo apt install nginx -y
```

### Verification

The installation was verified by accessing the server through a web browser using the public IP address:

```text
http://20.5.17.115
```

The default Nginx welcome page was displayed successfully, confirming that the web server was operating correctly.

# WordPress Deployment and Configuration

## Downloading WordPress

The latest WordPress package was downloaded from the official website.

```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
```

The package was extracted:

```bash
tar -xvzf latest.tar.gz
```

This created a WordPress directory containing all required website files.

---

## Backing Up Existing Website Files

Before replacing the existing website, a backup was created.

```bash
sudo mkdir -p /home/vaibhavsaini/website-backup
sudo cp -r /var/www/html/* /home/vaibhavsaini/website-backup/
```

This ensured that the original website could be restored if any issues occurred during deployment.

---

## Deploying WordPress Files

The extracted WordPress files were copied to the Nginx web root directory.

```bash
sudo cp -r /tmp/wordpress/* /var/www/html/
```

Ownership and permissions were updated:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo find /var/www/html -type d -exec chmod 755 {} \;
sudo find /var/www/html -type f -exec chmod 644 {} \;
```

Verification:

```bash
ls /var/www/html
```

The output confirmed that WordPress files such as wp-admin, wp-content and wp-includes were successfully deployed.

---

## Installing MariaDB

WordPress requires a database to store website content, users, settings, and blog posts.

MariaDB was installed:

```bash
sudo apt install mariadb-server -y
```

The database service was verified:

```bash
sudo systemctl status mariadb
```

The service was confirmed to be running successfully.

---

## Creating the WordPress Database

MariaDB was accessed:

```bash
sudo mysql
```

A new database was created:

```sql
CREATE DATABASE wordpress;
```

A dedicated WordPress user account was created:

```sql
CREATE USER 'wpuser'@'localhost'
IDENTIFIED BY 'StrongPassword123!';
```

Permissions were granted:

```sql
GRANT ALL PRIVILEGES
ON wordpress.*
TO 'wpuser'@'localhost';
```

Privileges were refreshed:

```sql
FLUSH PRIVILEGES;
```

The database was verified using:

```sql
SHOW DATABASES;
```

The newly created WordPress database appeared in the list.

---

## Installing PHP

Since WordPress is a PHP-based application, PHP and PHP-FPM were required.

Installation:

```bash
sudo apt install php-fpm php-mysql -y
```

Verification:

```bash
php -v
```

The server was running PHP 8.3.

The PHP-FPM service was checked:

```bash
sudo systemctl status php8.3-fpm
```

The service was active and running successfully.

---

## Configuring WordPress

The WordPress sample configuration file was copied:

```bash
sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
```

The configuration file was edited:

```bash
sudo nano /var/www/html/wp-config.php
```

Database details were entered:

```php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'StrongPassword123!');
define('DB_HOST', 'localhost');
```

Verification:

```bash
grep "DB_" /var/www/html/wp-config.php
```

The correct database credentials were displayed.

---

## Configuring Nginx for WordPress

The Nginx default configuration file was edited:

```bash
sudo nano /etc/nginx/sites-enabled/default
```

The index order was updated:

```nginx
index index.php index.html index.htm;
```

The WordPress routing rule was added:

```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}
```

PHP processing support was enabled:

```nginx
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php8.3-fpm.sock;
}
```

---

## Testing and Troubleshooting

Configuration testing:

```bash
sudo nginx -t
```

Output:

```text
nginx: configuration file test is successful
```

Nginx was reloaded:

```bash
sudo systemctl reload nginx
```

Several checks were performed to ensure WordPress was functioning correctly:

```bash
curl -I https://vaibhavblogs.com
```

The response showed:

```text
Location: https://vaibhavblogs.com/wp-admin/install.php
```

This confirmed that WordPress was correctly communicating with the database and redirecting users to the installation wizard.

---

## Completing WordPress Installation

The WordPress installation page was accessed through a web browser.

The following information was configured:

* Website Title
* Administrator Username
* Administrator Password
* Administrator Email Address

After installation, the WordPress dashboard became available.

---

## Result

WordPress was successfully deployed on the Ubuntu server using Nginx, PHP 8.3, and MariaDB. The website became fully operational, allowing blog posts, pages, and website content to be managed through the WordPress administration dashboard.

