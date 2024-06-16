
# Project title: Static website deployment on AWS

This project will illustrate how to deploy a static website on AWS platform.

Prerequisite:
AWS account
Basic linux skills
Simple static website with html and CSS


Steps
1. Static website creation.
Create a simple static website. The website for `Staples foodShop` is found within this github repo

2. Set up AWS environment.
We will be deploying the static website on an NGINX webserver on an AWS EC2 instance. 
- First, create an EC2 instance on AWS in the default VPC (for simplicity) with the following parameters:
name: staples-foodShop
image: Amazon Linux 2
instance type: t2-micro

For the security group settings, we will open the ports on ports 80, 443 and 22

3. Connect to the instance from your terminal using ssh client or cloudshell

4. Install and configure NGINX.

Note that we can alternatively input the userdata with a bashscript to create nginx when we are launching our instance.

5. Copy website files to the EC2 instance

6. Configure NGINX to serve the static website

7. Access the website from browser using the public IP



