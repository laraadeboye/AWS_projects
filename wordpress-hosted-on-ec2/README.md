# Creating and Hosting a WordPress website on an EC2 instance

This project will set up a WordPress website on an Amazon Elastic Compute Cloud (EC2) instance. Hosting a website on EC2 provides the following benefits:

* Customization: Gain full control over the server environment and website configuration.
* Scalability: Easily scale resources (CPU, memory) as website traffic grows.
* Cost-efficiency: Pay only for the resources you use

**Problem:** ExampleOrganicsCompany, a local farmers market, lacked an online presence to showcase their products and connect with customers. They needed a user-friendly platform to manage their product catalog, receive online orders, and promote their offerings.

**My Role:** As a Cloud Architect, I will design and implement a scalable solution using AWS EC2. Launching an EC2 instance with appropriate resources, I configure a LAMP stack (Linux, Apache, MySQL, PHP) to run WordPress, and install the WooCommerce plugin for e-commerce functionality.

&nbsp;

## Overview
WordPress is an open-source content management system that is written in PHP. It relies on LAMP stack for its operation. LAMP stands for Linux, Apache (or Nginx), MYSQL and PHP respectively. 
A webserver is a software that listens for requests on specific ports (usually 80 for Http traffic) from a browser and delivers contents. Examples of webservers are Apache, Nginx, Microsoft IIS.

When a webserver receives  a request, it locates the appropriate response on the filesystem of the server, processes it by using scripting languages such as PHP and sends the appropriate response to the web browser.

Typically, a DevOps engineer is responsible for server management, to ensure that the LAMP stack is properly configured and maintained; security hardening to protect the website form vulnerabilities; VersionControl and Deployments and Performance optimization, to ensure fast loading times and a smooth user experience.

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
* **Key pair**: Create new key pair named **wp-key**
* **Network**: Use Default VPC
Under Network, **Create new security group:** (Allow SSH from port 22, Allow Http from port 80 and HTTPS from port 443)
* For the remaining configurations, accept default and Create the instance



Alternatively we can launch EC2 using the AWS CLI with the AWS CLI using the command below (Ensure to apply the correct parameters)

#
    aws ec2 run-instances --image-id <ami-xxxxxxxx> --count 1 --instance-type t2.micro --key-name <MyKeyPair> --security-group-ids <sg-903004f8> --subnet-id <subnet-6e7f829e>

&nbsp;

2. Assign an Elastic IP address. An Elastic IP is a public IPv4 address helps to ensure that the IP address of our server does not change if we stop or reboot our instance. It allows us to maintain a consistent connectivity to our resource. ( Note that AWS charges a small fee for the use of an Elastic IP). 


## Step 2. Install prerequisites. 

1. We will install an Apache webserver on our virtual machine.

We should have ssh-ed into the machine. We will install the Apache web server and SQL on our Ubuntu machine.

1. First update the VM:
#
    sudo apt update -y

2. Install Apache server on Ubuntu:
#
    sudo apt install apache2 -y

To verify the installation of the Apache server, visit the public IP of the instance on a web browser. (Note: Use http, not https)

#
    http://[PUBLIC_IP]

[IMAGE SAVED FOR UPLOAD]


3. Word press is based on php and mysql runtime connector. Install php runtime and php mysql connector:
#
    sudo apt install php libapache2-mod-php php-mysql -y

4. Install MYSQL server
#
    sudo apt install mysql-server -y

&nbsp;

##  Step 3. Configure MYSQL server and create a new database for wordpress

1. Login to MySQL server
#
    sudo mysql -u root

2. Change authentication plugin to a mysql_native_password. mysql_native_password is a more secure authetication plugin compared to older options. For compatibility sake, newer versions of MYSQL being used with newer versions of WordPress require mysql_native_password. It is important to use a [strong password](https://support.microsoft.com/en-us/windows/create-and-use-strong-passwords-c5cebb49-8c53-4f5e-2bc4-fe357ca048eb#:~:text=At%20least%2012%20characters%20long,different%20from%20your%20previous%20passwords.). 

*TROUBLESHOOTING TIP*:
*If you're encountering issues during installation and are using a newer MySQL version, verify the authentication plugin is set to mysql_native_password.*

#
    ALTER USER 'root'@localhost IDENTIFIED WITH mysql_native_password BY '<newpassword>';

3. Create a new database user for wordpress  named **wp_user**(Remember to use a strong password)
#
    CREATE USER 'wp_user'@localhost IDENTIFIED BY '<strong password>'; 

4. Create a new database called **wp**
#
    CREATE DATABASE wp;

5.  Grant all privileges on the database 'wp' to the newly created user. (Note the regex in the command)
#
    GRANT ALL PRIVILEGES ON wp.* TO 'wp_user'@localhost;

6. Exit the MYSQL server to return to the ubuntu wordpress-server terminal
#
    exit 
or exit with keyboard shortcut Ctrl+D

Note that the database configuration details should be saved as they will be used in a later step.

&nbsp;

##   Step 4. Install and Configure wordPress.
Visit [wordpress.org](https://wordpress.org/download/releases/) to get the link of the latest archive file for download

1. Download wordPress into the /tmp directory by running the following commands:
#
    cd /tmp
#
    wget https://wordpress.org/latest.tar.gz 

The above command will download the latest wordPress release.

Run the ls commant to confirm that the archive has been downloaded.
#
    ls


2. Unzip the archive file using the tar utility
#
    tar -xvf latest.tar.gz

Running the ls command again will show a new folder called **wordpress** in the current directory

3. We will move the wordpress folder to the apache document root
#
    sudo mv wordpress/ /var/www/html

4. Verify the move by listing the /var/www/html directory:
#
    ls /var/www/html
    SS
5. Navigate to your browser and type:
#
    http://[PUBLIC_IP]/wordpress

This will bring us to the wordPress installation page  as shown:

[IMAGE AVAILABLE FOR UPLOAD]

6. We will configure the database on the wordpress installation page using the following details. (Note: the details were used to configure MYSQL)

**Database Name**: wp
**Username**: wp_user
**password**: <the strong password>
**host**: local host

Click **Submit**

We may see the following error:

[UPLOAD WP ERROR MESSAGE]

We will rectify the error by manually creating the wp-config.php file. Copy the PHP script in the dialogue box, and run the following commands to create a text editor named `wp-config.php` paste the script onto the text editor and save the file.

#
    cd /var/www/html/wordpress

#
    vi wp-config.php

Then click **run the installation**

We will be taken to the next page, where we ae requred to fill details for the website.(Be mindful to use a strong password)
[IMAGE UPLOAD]
[IMAGE UPLOAD FILLED DETAILS]

After filling the required details, Click **Install Wordpress**
You will get the success page from where you can log in to wordpress after filling the required username and password.SS

[IMAGE UPLOAD]



&nbsp;

## Step 5. Configure wordpress website to be served on the root path
The root path, currently serves the Apache2 default page. Wordpress can be configured to be accessible on the root path. This can be achieved by modifying the apache2 configuration in the /etc/apache2/sites-available/

#
    cd /etc/apache2/sites-available/

Listing the directory will reveal the following files:
000-default.conf  default-ssl.conf

The file we need to modify is the `000-default.conf` file. Use a text editor to open it:

#
    sudo vi 000-default.conf

Modify the Documentroot as follows:

`DocumentRoot /var/www/html/wordpress`


We will restart the apache web server 

#
    sudo systemctl restart apache2

&nbsp;    

## Step 6. Link a domain to the website.
We can either use Route 53 (AWS DNS) or purchase from a host.
Set the the domain name as A record.
You can set the TTL to 60seconds
Point the the domain name to your elastic public IPv4 address.

Modify your apache configuration file to contain the following:
* ServerName examplewebsite.com (this is your domain name)
* SeverAlias www.examplewebsite.com

Remember to save the file and restart Apache2


To complete this settings, visit your wordpress website, login and navigate to settings, then general. Change the WordPress Address URL and site Address URL to your domain name. Then click **Save** 

## Step 7. Install SSL on the Website (Make the website secure)
1. To install SSL on our website to secure it, we will first update the webserver, then install certbox with the following commands:
#
    sudo apt-get update
    sudo apt install certbot python3-certbot-apache

2. Install ssl on the website with certbox
#
    sudo certbot --apache

Follow the prompt and fill the correct details

## Conclusion