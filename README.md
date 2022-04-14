# Nginx forward proxy

[Nginx](https://nginx.org/en/) is a very fast HTTP and reverse proxy server. 
Usually, Nginx is used to serve and cache static assets or as proxy or load balancer for incoming traffic to application servers. In this repository, it is used as forward proxy.  This repository uses the third party ngx_http_proxy_connect (https://github.com/chobits/ngx_http_proxy_connect_module) repository to enable forward HTTPS proxy.  A copy of that repository is included in this one as a backup.

## Use Case

This proxy is used for the Agilysys integration so that all outgoing API calls come from a static IP address.  The static IP address is configured in AWS Elastic IP (agilysys-proxy-server-static-ip).  The python API calls must also be configured to send through the proxy server for this to be used correctly.

## Running Proxy Server
```
# From this git directory, Start the Docker container
docker-compose up -d
```

## Setting up the ec2 instance

If anything goes wrong with the proxy server, we should simply be able to restart the Docker container.  If the ec2 needs to be re-created though, you can follow these commands and steps to get set up.  Note that port 8888 on the ec2 instance needs to allow all TCP connections for this proxy to be accessible.

```
# Add your email instead of the placeholder.  Enter and confirm your password when prompted
ssh-keygen -t ed25519 -C {your_email_here}

# Copy SSH public key to clipboard
cat ~/.ssh/id_ed25519.pub

# Add SSH public key to your github public keys so that repository can be pulled
git clone git@github.com:BbotLLC/nginx_proxy.git

# Install Docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Edit $http_x_proxy_auth != "" in nginx.conf (the one in this repo) to include the Agilysys proxy header secret.
# cm-django-prod-agilysys-proxy-header-secret in AWS Parameter Store
```
