title: nginx connection limit bandwith limit rate limit
categories: web
tags: [nginx]
date: 2017-02-13 16:30:57
---

## connection limit
每一ip最多50連線數
``` sh
limit_conn_zone $binary_remote_addr zone=connlimit:10m;
server {
    # ...
    location / {
      limit_conn connlimit 50;
      # ...
    }
    # ...
 }

```

## bandwith limit
當流量超過500k時限制速度為50k
``` sh
limit_conn_zone $binary_remote_addr zone=connlimit:10m;

server {
    # ...
    location / {
      limit_rate 50k;
      limit_rate_after 500k;
      # ...
    }
 }

```

## rate limit
每秒最多50 request
``` sh
limit_req_zone $binary_remote_addr zone=one:10m rate=50r/s;

server {
    # ...
    location / {
      limit_req zone=one burst=5 nodelay;
      # ...
    }
    # ...
 }

```
