# AltschoolProject1 - Hosting A Web Page Using AWS Instance/Console
Simple Web Server with HTTPS Configuration
This project demonstrates how to set up a basic web server on an AWS EC2 instance, deploy a simple HTML landing page, and secure the website using HTTPS with a free SSL certificate from Let's Encrypt. The goal is to make the web server accessible publicly via a domain name and ensure secure communication using SSL/TLS.

Project Overview
The project includes the following tasks:

Provisioning a Linux-based virtual server (EC2 instance) on AWS.
Installing and configuring the Apache web server to serve web content.
Deploying a custom HTML landing page to showcase the server's functionality.
Configuring DNS to link a domain name to the server’s public IP address.
Setting up HTTPS by installing a free SSL certificate from Let's Encrypt using Certbot.
Securing the server by enabling a firewall and ensuring only necessary traffic is allowed.
Verifying that both the HTTP and HTTPS versions of the site are functional and accessible.
This README provides detailed steps and commands, making it easy for anyone—even beginners—to replicate the process.

Steps to Complete the Project
Step 1: Provisioning the EC2 Instance
Log in to the AWS Management Console:

Navigate to the EC2 Dashboard.
Launch a New EC2 Instance:

Select Ubuntu Server 20.04 LTS as the Amazon Machine Image (AMI).
Choose the instance type: t2.micro (eligible for the free tier).
Configure instance details (leave default settings).
Add storage (default 8 GiB is sufficient).
Configure the security group:
Add the following rules:
SSH (Port 22) – Source: Your IP (for secure access).
HTTP (Port 80) – Source: 0.0.0.0/0 (for public web traffic).
HTTPS (Port 443) – Source: 0.0.0.0/0 (for secure web traffic).
Launch the Instance:

Download the key pair (.pem file) if this is your first time creating an instance.
Save the key pair securely as it is needed to connect to the instance.
Connect to the Instance:

Use an SSH client like PuTTY (on Windows) or the terminal (on Linux/macOS).
For PuTTY, convert the .pem key file to .ppk using PuTTYgen.
Step 2: Install and Configure Apache
Update the System: Run the following command to update and upgrade the instance:



sudo apt update && sudo apt upgrade -y
Install Apache: Apache is a widely used open-source web server. Install it by running:



sudo apt install apache2 -y
Start and Enable Apache: Ensure the Apache web server is running and set to start on boot:



sudo systemctl start apache2
sudo systemctl enable apache2
Verify Apache Installation:

Open a browser and navigate to http://13.52.221.0 (replace 13.52.221.0 with your instance's public IP address).
You should see the Apache default page indicating the server is running.
Step 3: Deploy the HTML Landing Page
Navigate to the Web Directory: The default location for web files in Apache is /var/www/html. Change to this directory:



cd /var/www/html
Remove the Default Page: Delete the default index.html file:



sudo rm index.html
Create Your Custom HTML Page: Use a text editor like Nano to create a new index.html file:


sudo nano index.html
Add the following content (replace with your details):
html

<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Project</title>
</head>
<body>
    <h1>Welcome to Francis Etang's Landing Page</h1>
    <p>This is a simple web server deployed on AWS.</p>
</body>
</html>
Save the file (CTRL+O, then Enter) and exit (CTRL+X).
Test Your Custom Page:

Visit http://13.52.221.0 in your browser to view your custom HTML content.
Step 4: Configure HTTPS with Let's Encrypt
Install Certbot: Certbot is a tool that automates the process of obtaining and renewing SSL certificates. Install it with:


sudo apt install certbot python3-certbot-apache -y
Run Certbot to Obtain an SSL Certificate: Use Certbot to automatically configure SSL for your domain:

sudo certbot --apache
Enter your domain name(s) when prompted (e.g., francisetang.mooo.com and www.francisetang.mooo.com).
Certbot will validate your domain, install the SSL certificate, and configure Apache for HTTPS.
Redirect HTTP to HTTPS: Certbot will ask if you want to redirect all HTTP traffic to HTTPS. Select Yes.

Verify HTTPS:

Visit https://francisetang.mooo.com in your browser.
Ensure a padlock icon is visible, confirming the connection is secure.
Step 5: Secure the Server
Enable UFW Firewall: Use UFW to allow only necessary traffic:



sudo ufw allow 'Apache Full'
sudo ufw enable
Check AWS Security Group Rules:

Verify that the Security Group allows traffic on ports 22 (SSH), 80 (HTTP), and 443 (HTTPS).
Project Deliverables
Key Information
Public IP Address:

Example: http://13.52.221.0.
Domain Name:

Example: https://francisetang.mooo.com.
Screenshots
Include the following:


Screenshot of the website on http://13.52.221.0.

Screenshot of the website on https://francisetang.mooo.com.
Scripts and Commands Used
Here is a summary of all the commands used in the project:

System Updates


sudo apt update && sudo apt upgrade -y
Install Apache


sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
Deploy HTML Page


cd /var/www/html
sudo rm index.html
sudo nano index.html
Configure HTTPS


sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache
Enable Firewall


sudo ufw allow 'Apache Full'
sudo ufw enable
Expected Results
HTTP: Redirects automatically to HTTPS.
HTTPS: Securely serves your custom landing page with a padlock icon.
Notes and Troubleshooting
Ensure DNS records are updated to point to your public IP.
If HTTPS doesn't work, check the Apache configuration and Certbot logs.
Use tools like SSL Checker to verify your SSL setup.



