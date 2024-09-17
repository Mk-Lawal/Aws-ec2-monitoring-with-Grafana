

# AWS EC2 Monitoring with Grafana

This project demonstrates how to deploy Grafana on an AWS EC2 instance running Ubuntu 20.04 to set up a monitoring system. The README outlines the steps for installing Grafana, configuring it with Nginx as a reverse proxy, and accessing the Grafana dashboard.

## Project Overview

This project involves setting up Grafana for monitoring purposes on an AWS EC2 instance. Grafana is a popular open-source platform for monitoring and observability. This guide will walk you through the installation and configuration process.

![Grafana Diagram](https://github.com/Mk-Lawal/Aws-ec2-monitoring-with-Grafana/blob/bc9024e301a2633414f872b6b1fbf9dc44d1eb61/Grafana.png)

## Installation Steps

### 1. Update Ubuntu

```bash
sudo apt-get update -y
```

### 2. Install Necessary Packages

```bash
sudo apt-get install wget curl gnupg2 apt-transport-https software-properties-common -y
```

### 3. Add Grafana GPG Key

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

### 4. Add Grafana Repository

```bash
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

### 5. Refresh and Update Package List

```bash
sudo apt-get update -y
```

### 6. Install Grafana

```bash
sudo apt-get install grafana -y
```

### 7. Check Grafana Version

```bash
grafana-server -v
```

### 8. Start Grafana Service

```bash
sudo systemctl start grafana-server
```

### 9. Enable Grafana to Start on Boot

```bash
sudo systemctl enable grafana-server
```

### 10. Check Grafana Service Status

```bash
systemctl status grafana-server
```

### 11. Check Grafana Running Port

```bash
ss -antpl | grep 3000
```

### 12. Install Nginx

```bash
sudo apt-get install nginx -y
```

### 13. Configure Nginx as a Reverse Proxy

Create a new Nginx configuration file:

```bash
sudo nano /etc/nginx/conf.d/grafana.conf
```

Add the following configuration:

```nginx
server {
    server_name enter-your-ec2-dnsnamehere;
    listen 80;
    access_log /var/log/nginx/grafana.log;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $http_host;
        proxy_set_header X-Forwarded-Host $host:$server_port;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

### 14. Increase Nginx Bucket Size

Edit the Nginx configuration file:

```bash
sudo nano /etc/nginx/nginx.conf
```

Set `server_names_hash_bucket_size` to 128:

```nginx
http {
    ...
    server_names_hash_bucket_size 128;
    ...
}
```

### 15. Test Nginx Configuration

```bash
sudo nginx -t
```

### 16. Restart Nginx

```bash
sudo systemctl restart nginx
```

## Access Grafana

You can now access the Grafana dashboard from the public IP address of your EC2 instance. Use the default login credentials:
- **Username:** admin
- **Password:** admin

## Repository

For additional details and to view the Nagios diagram, visit the [GitHub repository](https://github.com/Mk-Lawal/Aws-ec2-monitoring-with-Grafana).

---

Feel free to adjust any details or add more information specific to your setup!
