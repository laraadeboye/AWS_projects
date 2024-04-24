# Creating and Hosting a WordPress website on an EC2 instance

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


&nbsp;

## Step 1. Launch an EC2 instance. 
You can launch an EC2 instance from the command line using AWS CLI or from the AWS console. To launch EC2 from the command line, the prerequisite is to download AWS CLI from the [AWS documentation](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-version.html)

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

&nbsp;

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

&nbsp;

##  Step 3. Configure MYSQL server and create a new database for wordpress

1. Login to MySQL server
#
    sudo mysql -u root

2. Change authentication plugin to mysql_native_password. It is important to use a [strong password](https://support.microsoft.com/en-us/windows/create-and-use-strong-passwords-c5cebb49-8c53-4f5e-2bc4-fe357ca048eb#:~:text=At%20least%2012%20characters%20long,different%20from%20your%20previous%20passwords.). 

#
    ALTER USER 'root'@localhost IDENTIFIED WITH mysql_native_password BY '<newpassword>';

3. Create a new database user for wordpress  named **wp_user**(Remember to use a strong password)
#
    CREATE USER 'wp_user'@localhost IDENTIFIED BY '<strong password>'; 

4. Create a new database called **wp**
#
    CREATE DATABASE wp;

5.  Grant all privilges on the database 'wp' to the newly created user. (Note the regex in the command)
#
    GRANT ALL PRIVILEGES ON wp.* TO 'wp_user'@localhost;

6. Exit the MYSQL server to return to the ubuntu wordpress-server terminal
#
    exit

&nbsp;

##   Step 4. Install and Configure wordPress.

1. Download wordPress by running the following command:
#
    wget https://wordpress.org/latest.tar.gz -P /tmp

2. Unzip the archive file using the tar utility
#
    tar -xvf latest.tar.gz
3. We will move the wordpres folder to the apache root document
#
    sudo mv wordpress/ /var/www/html
4. We will restart the apache web server 
#
    sudo systemctl restart apache2
    

## Step 5. Install SSL on the Website.
1. To install SSL on our website to secure it, we will first update the webserver, then install certbox with the following commands:
#
    sudo apt-get update
    sudo apt install certbot python3-certbot-apache

2. Install ssl on the website with certbox
#
    sudo certbot --apache
## Conclusion