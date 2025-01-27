# Guide to Create an Nginx Server

## Description
In this guide, we will learn how to create an Nginx server on an Ubuntu machine without using Vagrant.

## Prerequisites
- An Ubuntu machine.
- Access to an account with sudo privileges.

## Steps to Follow

### 1. Update the System
First, we will update the list of available packages and upgrade the installed packages to their latest versions.

```bash
sudo apt-get update
sudo apt-get upgrade -y
```

### 2. Install Nginx
We will install Nginx using the apt package manager.


```bash
sudo apt-get install nginx -y
```

### 3. Start and Enable Nginx
We will start the Nginx service and enable it to start automatically on system boot.
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 4. Configure the Firewall
If you have ufw (Uncomplicated Firewall) enabled, you will need to allow HTTP and HTTPS traffic.
```bash
sudo ufw allow 'Nginx Full'
```

### 5. Verify the Installation
To verify that Nginx has been installed correctly, open a web browser and navigate to your server's IP address. You should see the Nginx welcome page.
```bash
http://<your_ip_address>
```

### 6. Configure a Website
We will create a basic configuration for a website.


#### 6.1 Create the Website Directory
Create a directory for your website in /var/www.

```
sudo mkdir -p /var/www/my_website/html
sudo chown -R $USER:$USER /var/www/my_website/html
sudo chmod -R 755 /var/www/my_website
```

#### 6.2 Create a Home Page
Create an index.html file in the website directory.

```bash
nano /var/www/my_website/html/index.html
```
Add the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to My Website</title>
</head>
<body>
    <h1>Hello World!</h1>
    <p>This is my website served by Nginx.</p>
</body>
</html>
```

#### 6.3 Create a Configuration File for the Site
Create a configuration file for your site in /etc/nginx/sites-available.
```bash
sudo nano /etc/nginx/sites-available/my_website
```

Add the following content:
```nginx
server {
    listen 80;
    server_name example.com;

    root /var/www/my_website/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

#### 6.4 Enable the Site
Create a symbolic link in the sites-enabled directory.
```bash
sudo ln -s /etc/nginx/sites-available/my_website /etc/nginx/sites-enabled/
```

#### 6.5 Test the Nginx Configuration
Test the Nginx configuration to ensure there are no syntax errors.
```bash
sudo nginx -t
```

#### 6.6 Restart Nginx
Restart Nginx to apply the changes.
```bash
sudo systemctl restart nginx
```

### 7. Access Your Website
Open a web browser and navigate to your server's IP address or your domain. You should see the home page you created.
```bash
http://<your_ip_address> or http://example.com
```
