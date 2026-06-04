# ICT171-ASSIGNMENT3-VS
Name- Vaibhav Saini
Student ID- 35819283
## Current Development Stage

This project is currently in the initial planning and preparation stage. At this phase, the core project idea, server requirements, and implementation approach are being carefully planned and evaluated. Initial cloud server deployment and basic configuration have been completed to establish a foundation for future development.

Further research, testing, and dry-run implementations will be conducted before the final production setup is developed. Upcoming stages of the project will focus on improving the website structure, implementing additional server functionality, configuring DNS and SSL/TLS security, developing useful scripts, and progressively expanding the overall functionality and documentation of the system.
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


The default Nginx welcome page was displayed successfully, confirming that the web server was operating correctly.

```
```
