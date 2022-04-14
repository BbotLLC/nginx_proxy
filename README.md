## Use Case

This proxy is used for the Agilysys integration so that all outgoing API calls come from a static IP address.  The static IP address is configured in AWS Elastic IP (agilysys-proxy-server-static-ip).  The python API calls must also be configured to send through the proxy server for this to be used correctly, although this should already be done.

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

# Edit $http_x_proxy_auth != "" in nginx.conf on the ec2 to include the Agilysys proxy header secret.
# Note that this file is included in this repo but the edit should not be comitted to source control.
# cm-django-prod-agilysys-proxy-header-secret is the parameter name in AWS Parameter Store
```

## Authorization
This proxy uses basic secret key authentication to restrict access.  The X-PROXY-AUTH header should be included in proxied HTTP requests for validation.  The secret is stored in AWS parameter store and is inserted into the nginx.conf file on the ec2 server (instructions for that above).

## Python Setup

Code changes in Python should not be required to get Agilysys to work with this proxy server.  In the case they are needed, however, all API requests to Agilysys services should be made through this proxy.
