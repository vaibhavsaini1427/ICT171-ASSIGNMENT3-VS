# ICT171-ASSIGNMENT3-VS

**Name:** Vaibhav Saini

**Student ID:** 35819283

**Public IP Address:** http://20.5.17.115/

**Domain Name:** https://vaibhavblogs.com/


## Project Overview

The objective of this project was to design, deploy, and manage a cloud-hosted server environment using Microsoft Azure. An Ubuntu Linux virtual machine was configured to host a WordPress blogging platform using Nginx, PHP, and MariaDB.

A custom domain name (vaibhavblogs.com) was integrated using DNS records, and HTTPS encryption was implemented using Let's Encrypt SSL/TLS certificates. In addition to the website deployment, a custom Bash script was developed to automate WordPress backups and generate a web-based server status report.

The project demonstrates cloud server deployment, web server administration, DNS management, SSL/TLS implementation, WordPress hosting, Linux system administration, and Bash scripting.

## Microsoft Azure Virtual Machine Deployment

A Linux virtual machine was deployed using Microsoft Azure Infrastructure as a Service (IaaS). The virtual machine provides the hosting environment for the cloud server project and serves as the foundation for all deployed services.


### Virtual Machine Configuration

| Component         | Configuration   |
| ----------------- | --------------- |
| Cloud Provider    | Microsoft Azure |
| Operating System  | Ubuntu Server   |
| Public IP Address | 20.5.17.115     |
| Access Method     | SSH             |


The virtual machine was created through the Azure Portal and assigned a public IP address to allow remote administration and public access to hosted services.

![Azure VM Overview](images/Screenshot%202026-06-05%20210149.png)
![Azure Virtual Machine Overview](images/Screenshot%202026-06-05%20210800.png)
### Verification

The successful deployment of the virtual machine was verified by connecting to the server remotely using SSH from a Windows workstation.

## Ubuntu Server Configuration

Ubuntu Server was selected as the operating system due to its stability, security, and widespread use in cloud environments.

Remote administration was performed through SSH, allowing secure command-line access to the server.

### SSH Connection Command

```bash
ssh -i "vsaini14711_key (1).pem" vaibhavsaini@20.5.17.115
```

### Verification

Successful login to the Ubuntu server confirmed that network connectivity and remote administration were functioning correctly.

## Nginx Web Server Installation

Nginx was selected as the web server software due to its high performance, reliability, and widespread use in modern web hosting environments. The web server is responsible for handling incoming HTTP and HTTPS requests and serving website content to users.

### Commands Used

```bash
sudo apt update
sudo apt install nginx -y
```

### Verification

The installation was verified by accessing the server through a web browser using the public IP address:

http://20.5.17.115

The default Nginx welcome page was displayed successfully, confirming that the web server was operating correctly and accessible from the Internet.

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
![DNS Configuration Screenshot 1](images/Screenshot%202026-05-19%20120027.png)
#### CNAME Record
- Type: CNAME
- Host/Name: www
- Points to/Value: vaibhavblogs.com
- TTL: 1/2 HOUR
![DNS Configuration Screenshot 2](images/Screenshot%202026-05-19%20120527.png)
### Verification

DNS propagation was verified by successfully accessing the website through the custom domain name:

https://vaibhavblogs.com

The successful connection confirmed that the domain name was correctly resolving to the Azure virtual machine.
## SSL/TLS HTTPS Configuration

SSL/TLS encryption was implemented using Let's Encrypt and Certbot to improve the security and accessibility of the cloud-hosted web server.

The Certbot package for Nginx was installed on the Ubuntu virtual machine, and a trusted SSL certificate was generated for both the primary domain and the www subdomain.

### Commands Used

```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d vaibhavblogs.com -d www.vaibhavblogs.com
```
### Verification

The SSL certificate was successfully installed and HTTPS became active on the website.

The secure connection was verified by accessing:

https://vaibhavblogs.com

A browser padlock icon confirmed that encrypted HTTPS communication was functioning correctly.

After doing these steps the encrption was done






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
IDENTIFIED BY 'Password!';
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
define('DB_PASSWORD', 'Password!');
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

### Verification

The WordPress installation was successfully completed through the web-based installation wizard. The website became publicly accessible through the configured domain name and could be managed through the WordPress administration dashboard.

Website URL:

https://vaibhavblogs.com

WordPress Dashboard:
![WordPress Dashboard](images/Screenshot%202026-06-05%20211350.png)
https://vaibhavblogs.com/wp-admin
## Result

WordPress was successfully deployed on the Ubuntu server using Nginx, PHP 8.3, and MariaDB. The website became fully operational, allowing blog posts, pages, and website content to be managed through the WordPress administration dashboard.

## Blog Website Functionality

The deployed WordPress platform was configured as a blogging website. Multiple blog posts were created and published to demonstrate the functionality of the content management system.

The website allows administrators to create, edit, publish, and manage blog content through the WordPress dashboard. Visitors can access blog posts through the public website using a standard web browser.

### Features Demonstrated

- Blog post creation
- Content editing
- Public website access
- WordPress dashboard administration
- Dynamic content management

### Verification

The website is publicly accessible at:

https://vaibhavblogs.com

Several sample blog posts were successfully published and displayed through the website interface.

## Backup and Server Status Script

### Purpose

As part of the scripting component of this project, a custom Bash shell script was developed to automate WordPress backups and provide server status monitoring.

The script was designed to improve server administration by automatically backing up the WordPress database and website files while also generating a web-accessible report containing important server information.

This script demonstrates Linux automation, server management, and Bash scripting skills.

### Script Features

The script performs the following tasks:

- Creates a backup of the WordPress MariaDB database
- Creates a compressed backup of the website files
- Generates a web-based backup report
- Reports database backup status
- Reports website backup status
- Displays backup file sizes
- Displays total backup count
- Displays server uptime
- Displays available disk space
- Displays Nginx service status
- Displays MariaDB service status
- Displays PHP version information
- Displays WordPress file count

### Script Language

The script was developed using Bash Shell Scripting on Ubuntu Linux.

### Script Execution

The script can be executed using:

```bash
sudo bash ~/backup.sh
```

### Output

Upon execution, the script creates:

- Database backup files (.sql)
- Website backup archives (.tar.gz)
- A web-based backup report

Backup files are stored in:

```text
/home/vaibhavsaini/backups
```

### Verification

The script output can be independently verified through the following URL:

https://vaibhavblogs.com/backup-report.html

This page is automatically generated by the script and displays the latest backup status and server information.
![Backup Script Execution](images/Screenshot%202026-06-05%20211826.png)

### Result

The script successfully automated routine server administration tasks while providing a web-accessible verification page. This improved the maintainability of the WordPress server and demonstrated the use of Bash scripting for cloud server management.

### Script Source Code
```bash
#!/bin/bash

# Generate current date and time for backup filenames
DATE=$(date +"%Y-%m-%d_%H-%M-%S")

# Define important project directories
BACKUP_DIR="/home/vaibhavsaini/backups"
WEB_DIR="/var/www/html"
REPORT_FILE="$WEB_DIR/backup-report.html"

# Create backup directory if it does not exist
mkdir -p "$BACKUP_DIR"

# Define backup file names
DB_FILE="$BACKUP_DIR/db_backup_$DATE.sql"
SITE_FILE="$BACKUP_DIR/site_backup_$DATE.tar.gz"

# Create MariaDB database backup
mysqldump -u wpuser -p'Password!' wordpress > "$DB_FILE"
DB_STATUS=$?

# Create website file backup
tar -czf "$SITE_FILE" "$WEB_DIR"
SITE_STATUS=$?

# Collect backup statistics
DB_SIZE=$(du -h "$DB_FILE" | cut -f1)
SITE_SIZE=$(du -h "$SITE_FILE" | cut -f1)
TOTAL_BACKUPS=$(ls "$BACKUP_DIR" | wc -l)

# Collect server information
SERVER_NAME="vaibhavblogs.com"
CURRENT_USER="vaibhavsaini"
UPTIME_INFO=$(uptime -p)
DISK_SPACE=$(df -h / | awk 'NR==2 {print $4}')

# Check service status
NGINX_STATUS=$(systemctl is-active nginx)
MARIADB_STATUS=$(systemctl is-active mariadb)
PHP_VERSION=$(php -v | head -n 1)

# Count WordPress files
WP_FILE_COUNT=$(find "$WEB_DIR" | wc -l)

# Determine backup success status
if [ $DB_STATUS -eq 0 ]; then
    DB_RESULT="Successful"
else
    DB_RESULT="Failed"
fi

if [ $SITE_STATUS -eq 0 ]; then
    SITE_RESULT="Successful"
else
    SITE_RESULT="Failed"
fi

# Generate web-based backup report
cat > "$REPORT_FILE" << REPORT
<html>
<head>
<title>WordPress Backup Report</title>
</head>
<body>

<h1>WordPress Backup and Server Status Report</h1>

<h2>Backup Information</h2>
<p><strong>Backup Date:</strong> $DATE</p>
<p><strong>Database Backup:</strong> $DB_RESULT</p>
<p><strong>Website Backup:</strong> $SITE_RESULT</p>
<p><strong>Database Backup Size:</strong> $DB_SIZE</p>
<p><strong>Website Backup Size:</strong> $SITE_SIZE</p>
<p><strong>Total Backup Files:</strong> $TOTAL_BACKUPS</p>

<h2>Server Information</h2>
<p><strong>Server Name:</strong> $SERVER_NAME</p>
<p><strong>Current User:</strong> $CURRENT_USER</p>
<p><strong>Server Uptime:</strong> $UPTIME_INFO</p>
<p><strong>Available Disk Space:</strong> $DISK_SPACE</p>

<h2>Service Status</h2>
<p><strong>Nginx Status:</strong> $NGINX_STATUS</p>
<p><strong>MariaDB Status:</strong> $MARIADB_STATUS</p>
<p><strong>PHP Version:</strong> $PHP_VERSION</p>
<p><strong>WordPress File Count:</strong> $WP_FILE_COUNT</p>

</body>
</html>
REPORT
echo "Backup and server report completed successfully."
echo "Report available at: https://vaibhavblogs.com/backup-report.html"
```
## Verification URLs

### Main Website
https://vaibhavblogs.com

### WordPress Dashboard
https://vaibhavblogs.com/wp-admin

### Backup Report
https://vaibhavblogs.com/backup-report.html

## References

- Microsoft Azure Documentation
- Ubuntu Documentation
- Nginx Documentation
- MariaDB Documentation
- PHP Documentation
- WordPress Documentation
- Certbot Documentation

## Conclusion

This project successfully demonstrated the deployment and management of a cloud-hosted WordPress blogging platform using Microsoft Azure, Ubuntu Server, Nginx, PHP, and MariaDB. DNS and SSL/TLS were configured to provide secure public access through a custom domain name. In addition, a custom Bash script was developed to automate WordPress backups and generate a web-based server status report. The project provided practical experience in cloud infrastructure deployment, Linux server administration, web hosting, security configuration, and automation through scripting.
