title: Nginx With Node And Https config
categories: web
tags: [javascript, nodejs, https, ssl, web]
date: 2017-01-19 14:48:57
---
Nginx With Node And Https config example
<!--more-->

# nginx config examaple
pass all request to nodejs server

copy ssl cert files to /etc/nginx/ssl/
copy nginx.conf file to /etc/nginx/sites-available
and link to /etc/nginx/sites-enabled/
``` bash
cp ~/faceblock/faceblock-server/docker/faceblock/nginx.conf /etc/nginx/sites-available/faceblock
ln -s /etc/nginx/sites-available/faceblock /etc/nginx/sites-enabled/faceblock
```
``` bash
# nginx.conf
# the IP(s) on which your node server is running. I chose port 3001.
upstream app_faceblock {
    server 127.0.0.1:3001;
    keepalive 8;
}

limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s; # rate limit

# the nginx server instance
server {
    listen 0.0.0.0:80;
    server_name cwzc.pw www.cwzc.pw;
    access_log /var/log/nginx/faceblock.log;

    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    location /static {
      limit_req zone=one burst=5 nodelay;
      root /static;
    }

    # pass the request to the node.js server with the correct headers
    # and much more can be added, see nginx config options
    location / {
      limit_req zone=one burst=5 nodelay;
      
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;

      proxy_pass http://app_faceblock/;
      proxy_redirect off;
    }
 }
```

# Free SSL Certification
## Goto [sslforfree](https://www.sslforfree.com/) and get cert files
You will get three files ca_bundle.crt, certificate.crt, and private.key
## Copy files to /etc/nginx/ssl and do followings
``` bash
mv private.key nginx.key
#
cat certificate.crt ca_bundle.crt >> nginx.crt
vim nginx.crt
# change -----BEGIN-----END------ to
# -----BEGIN--------
# ----END--------
service nginx restart
```
## This free SSL Certification will expire after 90 days, so you will have to go to sslforfree to get new Certification before expired
