# Nginx forward proxy

[Nginx](https://nginx.org/en/) is a very fast HTTP and reverse proxy server. 
Usually, Nginx is used to serve and cache static assets or as proxy or load balancer for incoming traffic to application servers. In this repository, it is used as forward proxy.  This repository uses the third party ngx_http_proxy_connect (https://github.com/chobits/ngx_http_proxy_connect_module) repository to enable forward HTTPS proxy.  A copy of that repository is included in this one as a backup.

## Use Case

This proxy is used for the Agilysys integration so that all outgoing API calls come from a static IP address.  The static IP address is configured in AWS Elastic IP (agilysys-proxy-server-static-ip).  The python API calls must also be configured to send through the proxy server for this to be used correctly.

## Running Proxy Server
```
# generate ssh key
ssh-keygen -t ed25519 -C {your_email}
docker run -d -p 8888:8888 -v ${PWD}/nginx_whitelist.conf:/usr/local/nginx/conf/nginx.conf reiz/nginx_proxy:0.0.3 
```

Now the Docker container is running with the mounted configuration.

## Setting up the ec2 instance

If anything goes wrong with the proxy server, we should simply be able to restart the Docker container.  If the ec2 needs to be re-created though, you can follow these commands and steps to get set up.

```
# Add your email instead of the placeholder
ssh-keygen -t ed25519 -C {your_email_here}
# Enter password when prompted
# Confirm password

# Copy SSH public key to clipboard
cat ~/.ssh/id_ed25519.pub

# Add SSH public key to your github public keys so that repository can be pulled
git pull git@github.com:BbotLLC/nginx_proxy.git
```
