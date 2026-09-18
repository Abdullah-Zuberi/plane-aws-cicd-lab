# AWS EC2 Setup

This document describes how the EC2 instance was set up.

## Details
- **Region**: `us-east-1`
- **Instance Type**: `t3.micro` (AWS Free Tier Eligible)
- **AMI**: Ubuntu 22.04 LTS (ami-05a3e9423ae4d7a19)
- **Security Group**:
  - Inbound 22 (SSH) from `0.0.0.0/0`
  - Inbound 80 (HTTP) from `0.0.0.0/0`
- **SSH Key Name**: `github-actions-ec2-cicd`

## Preparation Commands
```bash
# Update OS
sudo apt-get update && sudo apt-get install -y curl nginx git

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs

# Install PM2 and pnpm
sudo npm i -g pm2 pnpm

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
```

## Reverse Proxy Setup (Nginx)
Configured to forward port 80 to localhost:3000 where the Next.js app runs.
```nginx
server { 
    listen 80; 
    server_name _; 
    location / { 
        proxy_pass http://localhost:3000; 
        proxy_http_version 1.1; 
        proxy_set_header Upgrade $http_upgrade; 
        proxy_set_header Connection "upgrade"; 
        proxy_set_header Host $host; 
        proxy_cache_bypass $http_upgrade; 
    } 
}
```
