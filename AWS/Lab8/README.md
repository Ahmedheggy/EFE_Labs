# AWS Apache Web Server Deployment

This guide outlines the steps to deploy a basic Apache web server on an AWS EC2 instance within a custom VPC. This project is part of a "Senior Steps - IT training center" lab.

## Deployment Steps

1.  **Create VPC:** Set up a VPC (10.0.0.0/16) with a public subnet (10.0.1.0/24), an Internet Gateway, and a public route table.

2.  **Launch EC2 Instance:** Launch a `t2.micro` Amazon Linux 2 instance into the public subnet. Ensure "Auto-assign Public IP" is enabled.

3.  **Configure Security Group:** Create a security group named `web-sg` allowing SSH (Port 22) from your IP and HTTP (Port 80) from anywhere.

4.  **Install Apache:** Connect to the instance via SSH and install the Apache web server (httpd).

5.  **Create Web Page:** Create a simple `index.html` file in the web root (`/var/www/html/`).

6.  **Access Website:** Open a web browser and navigate to the public IP address of your EC2 instance to view the website.