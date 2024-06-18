

# Project title: Static website deployment on AWS

This project will illustrate how to deploy a static website on AWS platform.

Prerequisite:
- AWS account
- Basic linux skills
- Simple static website with html and CSS


### Steps
1. **Static website creation.**
Create a simple static website. The website for `Staples foodShop` is found within this github [repo](https://github.com/laraadeboye/AWS_projects/tree/main/static-website-deploy-AWS/website-files).

2. **Set up AWS environment.**
We will be deploying the static website on an NGINX webserver on an AWS EC2 instance. 
- First, create an EC2 instance on AWS in the default VPC (for simplicity) with the following parameters:
- name: `staples-foodShop`
- image: `Amazon Linux 2`
- instance type: `t2-micro`

For Key pair name: Create a new key pair of type RSA named `stapleskey.pem`

For the security group settings, create a new security group named `staplesSG`. We will open the ports on ports 80 (http), 443 (https) and 22 (SSH).
The source type of port 22 should be `My IP`
The source type of ports 80 and 443 should be from `Anywhere`.

3. **Connect to the instance from your terminal using ssh client or cloudshell.**
Change the permisions on your the `stapleskey.pem` to allow only the user permission to read.
#
    chmod 400 stapleskey.pem

Connect to the instance by entering the following command, replacing [PUBLIC IP] with the public IP of the launched instance:

#
    ssh -i stapleskey.pem ec2-user@[PUBLIC IP]



4. **Install NGINX on the virtual machine.**

Note that we can alternatively input the userdata with a bashscript to create nginx when we are launching our instance.

First, update the linux machine:
#
    sudo yum update -y

Install nginx with the following command and verify the installation:

#
    sudo yum install nginx -y

    nginx -v

5. **Start NGINX service**
We can start NGINX with the following command:

#
    sudo systemctl start nginx

To enable nginx automatically start on reboot:

#
    sudo systemctl enable nginx

Verify nginx status:

#
    sudo systemctl status nginx

You can verify that NGINX is installed by copying the public IP and pasting it in a browser.

6. **Upload website files to the EC2 instance.**

First, make a directory for our website files in the /usr/share/nginx directory. 

#
    sudo mkdir -p /usr/share/nginx/staplesfoodShop

Set the permissions:
#
    sudo chown -R ec2-user:ec2-user /usr/share/nginx/staplesfoodShop

Change to the directory where your website files are located. Using scp, securely copy the website files from your local machine to the virtual machine. Replace [PUBLIC IP] with the public IP of the instance.
#
    scp -i /path/to/your-key-pair.pem /index.html styles.css ec2-user@[PUBLIC IP]:/usr/share/nginx/staplesfoodShop/


6. **Configure NGINX to serve the static website.**

The configuration files are stored in the `/etc/nginx` folder. We will edit the NGINX configuration file:

#
    sudo nano /etc/nginx/conf.d/staplesfoodShop.conf

Add the following configuration. Replace [PUBLIC IP] with your instance public IP:

```
server {
    listen 80;
    server_name [PUBLIC IP];

    root /usr/share/nginx/staplesfoodShop;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

```

Test the NGINX configuration:

#
    sudo nginx -t

Reload NGINX

#
    sudo systemctl reload nginx


7. **Access the website from browser using the public IP.**

Open your web browser and access the site at the public IP. Replace [PUBLIC IP] with the public IP of the instance.

#
    http://[PUBLIC IP]


&nbsp;

***Troubleshooting:***

*1. Review security group rules. Ensure that the inbound security group has your IP listed as your IP.If you are connecting to your instance from a linux machine, check your IP by entering the following command:*

#
    curl ifconfig.me
    
*2. On an ubuntu machine, your username is **ubuntu** while on an Amazon linux machine, your username is **ec2-user**.*
