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
