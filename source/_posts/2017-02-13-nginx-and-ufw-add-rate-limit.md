title: nginx and ufw add rate limit
categories: web
tags: [nginx, firewall, ubuntu]
date: 2017-02-13 16:30:57
---

production環境下需要設定rate limit來避免DOS攻擊  
以下介紹兩種設定方式，一種是讓ap server來擋(這裡介紹nginx)，一種是讓系統防火墻來擋(這裡介紹ufw)

<!--more-->

## nginx
將limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;加入nginx.conf最上一層  
將limit_req zone=one burst=5 nodelay;加入欲使用rate limit的location中  
nginx.fonf Ex:  
``` sh
# the IP(s) on which your node server is running. I chose port 3000.
upstream app_faceblock {
    server 127.0.0.1:3001;
    keepalive 8;
}

limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;

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

## ufw
ufw是ubuntu中的Filewall
可以讓我們自定各種rule
詳細解釋請參照https://gist.github.com/lavoiesl/3740917
修改/etc/ufw/user.rules
``` sh
### Add those lines after *filter near the beginning of the file
:ufw-http - [0:0]
:ufw-http-logdrop - [0:0]



### Add those lines near the end of the file

### Start HTTP ###

# Enter rule
-A ufw-before-input -p tcp --dport 80   -j ufw-http
-A ufw-before-input -p tcp --dport 443  -j ufw-http

# Limit connections per Class C
-A ufw-http -p tcp --syn -m connlimit --connlimit-above 50 --connlimit-mask 24 -j ufw-http-logdrop

# Limit connections per IP
-A ufw-http -m state --state NEW -m recent --name conn_per_ip --set
-A ufw-http -m state --state NEW -m recent --name conn_per_ip --update --seconds 10 --hitcount 20 -j ufw-http-logdrop

# Limit packets per IP
-A ufw-http -m recent --name pack_per_ip --set
-A ufw-http -m recent --name pack_per_ip --update --seconds 1  --hitcount 20  -j ufw-http-logdrop

# Finally accept
-A ufw-http -j ACCEPT

# Log-A ufw-http-logdrop -m limit --limit 3/min --limit-burst 10 -j LOG --log-prefix "[UFW HTTP DROP] "
-A ufw-http-logdrop -j DROP

### End HTTP ###
```

restart ufw
``` sh
ufw disable
ufw enable
```
