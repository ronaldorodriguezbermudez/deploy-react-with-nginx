Deploying a Web App on a Reverse Proxy Server
Ensure that NGINX is installed on your server. If you're unsure how to do this, you can refer to this guide: [guide](https://github.com/ronaldorodriguezbermudez/deploy-react-with-nginx/blob/main/Guide%20to%20Create%20an%20Nginx%20Server.md) for instructions.
---
## Environment Setup
1. Use a virtual machine with Ubuntu as the operating system.
2. Configure the machine's network to allow connectivity.
---
## Install NGINX
1. Update system packages:
```bash
sudo apt-get update
```
2. Install NGINX:
```bash
sudo apt-get install -y nginx
```
---
## Install Dependencies and Utilities
### Install NVM
1. Install nvm by running:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```
2. Load nvm into the terminal:
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
```
### Install Node.js and npm
1. Install Node.js version 16:
```bash
nvm install 16
```
2. Use the installed version:
```bash
nvm use 16
```
---
## Install Git
1. Install Git:
```bash
sudo apt install git
```
---
## Clone the Application
1. Download the application:
```bash
git clone https://github.com/docker/docker-nodejs-sample
```
---
## Run the Application with PM2
### What is PM2?
PM2 is a process manager for Node.js applications. It allows you to run, monitor, and manage your applications in production. PM2 provides features such as automatic restarts on crashes, clustering for performance improvement, log management, and more. It's widely used for managing Node.js applications and ensures they run smoothly in production environments.
1. Install PM2 globally:
```bash
npm install pm2 -g
```
2. Navigate to the project directory:
```bash
cd docker-nodejs-sample/
```
3. Start the application:
```bash
pm2 start src/index.js
```
4. Verify that the application is listening on port 3000:
```bash
lsof -i :3000
```
---
## Configure NGINX as a Reverse Proxy
1. Edit the NGINX configuration file:
```bash
sudo nano /etc/nginx/sites-available/default
```
2. Add the following configuration:
```nginx
server {
    listen 80;
    server_name 192.168.56.10;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
3. Check the configuration for syntax errors:
```bash
sudo nginx -t
```
4. Restart NGINX to apply the changes:
```bash
sudo systemctl restart nginx
```
---
## Test the Application
1. Open your browser and navigate to: `http://192.168.56.10/`
2. Confirm that the application is working correctly.
