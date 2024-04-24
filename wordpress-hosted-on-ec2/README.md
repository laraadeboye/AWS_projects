# Creating and Hosting a Wordpress website on an EC2 instance

This project will set up a WordPress website on an Amazon Elastic Compute Cloud (EC2) instance. Hosting a website on EC2 provides the following benefits:

* Customization: Gain full control over the server environment and website configuration.
* Scalability: Easily scale resources (CPU, memory) as website traffic grows.
* Cost-efficiency: Pay only for the resources you use

**Problem:** ExampleOrganicsCompany, a local farmers market, lacked an online presence to showcase their products and connect with customers. They needed a user-friendly platform to manage their product catalog, receive online orders, and promote their offerings.

**My Role:** As a Cloud Architect, I will design and implement a scalable solution using AWS EC2. Launching an EC2 instance with appropriate resources, I configure a LAMP stack (Linux, Apache, MySQL, PHP) to run WordPress, and install the WooCommerce plugin for e-commerce functionality.

## Outline
Step 0. Create a VPC

Step 1. Launch an EC2 instance.

Step 2. Install prerequisites.

Step 3. Configure MYSQL server and create a new database for wordpress

Step 4. Install and Configure wordPress

Step 5 Install SSL on the Website

Conclusion

&nbsp;

## Step 0. Create a VPC

It is important to create a new VPC for a project and not use the  existing default VPC provided by AWS. A VPC helps to isolate your projects resources and gives you granular control over the network environment for your project. 

For simplicity, We will use the default VPC to launch our website:

Check this [link](https://laraadeboye.hashnode.dev/how-to-configure-and-deploy-amazon-vpc-for-a-3-tier-web-app) for a reference to create a VPC for a three-tier web application.




## Step 1. Launch an EC2 instance. 
You can launch an EC2 instance from the command line using AWS CLI or from the AWS console. To launch EC2 from the command line, the prerequisite is to download AWS CLI from the [documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html)

Follow the steps to configure cmd (in windows) or terminal (in linux) to use AWS CLI

From the console: Create an EC2 instance with the following parameters:
* **Name**: wordpress-server
* **AMI**: Ubuntu Server 22.04 LTS (Free tier Eligible)
* **Instance type**: t2.micro
* **Key pair**: Create new key pair
* **Network**: Use Default VPC
Under Network, **Create new security group:** (Allow SSH from port 22, Allow Http from port 80 and HTTPS from port 443)
* For the remaining configurations, accept default and Create the instance



Alternatively we can launch EC2 using the AWS CLI with the AWS CLI using the command below (Ensure to apply the correct parameters)

#
    aws ec2 run-instances --image-id <ami-xxxxxxxx> --count 1 --instance-type t2.micro --key-name <MyKeyPair> --security-group-ids <sg-903004f8> --subnet-id <subnet-6e7f829e>


## Step 2. Install prerequisites. 

1. Install Apache server on the Ubuntu machine
We should have ssh-ed into the machine. We will install the Apache web server and SQL on our Ubuntu machine.

First update the VM:
#
    sudo apt update -y

Install Apache server on Ubuntu:
#
    sudo apt install apache2

Word press is based on php and mysql runtime connectorInstall php runtime and php mysql connector:
#
    sudo apt install php libapache2-mod-php php-mysql

Install MYSQL server
#
    sudo apt install mysql-server

##  Step 3. Configure MYSQL server and create a new database for wordpress
##   Step 4. Install and Configure wordPress.
## Step 5. Install SSL on the Website.
## Conclusion