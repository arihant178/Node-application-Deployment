
---

# Node.js Application Deployment with Nginx and PM2 on AWS

This repository provides a step-by-step guide to deploying a Node.js application on an AWS EC2 instance using Nginx as a reverse proxy and PM2 for process management.

## Prerequisites

- An AWS EC2 instance running Ubuntu.
- SSH access to the server.
- Basic knowledge of Node.js and Linux command-line operations.

## Deployment Steps

### 1. Update and Install Necessary Packages


```bash
sudo apt update
sudo apt install nginx nodejs npm ufw -y
```


### 2. Configure UFW Firewall


```bash
sudo ufw allow 'Nginx Full'
sudo ufw enable
```


### 3. Set Up the Application Directory


```bash
cd /var/www/html
sudo mkdir nodeapp
sudo chmod -R 777 nodeapp
cd nodeapp
```


### 4. Initialize Node.js Application


```bash
npm init -y
```


### 5. Create `index.js` File

Use a text editor to create and edit `index.js`:


```bash
nano index.js
```


Paste your Node.js application code into this file.

### 6. Install Dependencies


```bash
npm install express
npm install pm2 -g
```


### 7. Start the Application with PM2


```bash
pm2 start index.js
```


### 8. Configure Nginx as a Reverse Proxy

Create and edit the Nginx configuration file:


```bash
sudo nano /etc/nginx/sites-available/nodeapp
```


Add the following configuration:


```nginx
server {
    listen 80;
    server_name your_domain_or_IP;

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


Enable the configuration and restart Nginx:


```bash
sudo ln -s /etc/nginx/sites-available/nodeapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```


### 9. Enable Nginx to Start on Boot


```bash
sudo systemctl enable nginx
```


### 10. Access the Application

Visit `http://your_domain_or_IP` in your web browser to see the running Node.js application.

## Additional Notes

- Ensure that your security groups in AWS allow HTTP (port 80) and SSH (port 22) access.
- PM2 provides additional features like log management and monitoring. Refer to the [PM2 documentation](https://pm2.keymetrics.io/) for more details.

---

This `README.md` provides a clear and concise guide for deploying a Node.js application using Nginx and PM2 on an AWS EC2 instance. 
