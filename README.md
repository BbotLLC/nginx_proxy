# Nginx forward proxy

[Nginx](https://nginx.org/en/) is a very fast HTTP and reverse proxy server. 
Usually, Nginx is used to serve and cache static assets or as proxy or load balancer for incoming traffic to application servers. In this repository, it is used as forward proxy. 

## Use Case

This proxy is used for the Agilysys integration so that all outgoing API calls come from a static IP address.  The static IP address is configured in AWS Elastic IP (agilysys-proxy-server-static-ip).  The python API calls must also be configured to send through the proxy server for this to be used correctly.

## Running Proxy Server
pull this repository
```
docker run -d -p 8888:8888 -v ${PWD}/nginx_whitelist.conf:/usr/local/nginx/conf/nginx.conf reiz/nginx_proxy:0.0.3 
```

Now the Docker container is running with the mounted configuration.

