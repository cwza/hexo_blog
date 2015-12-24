title: Linux tool nsupdate筆記
categories: linux
tags: [linux]
date: 2015-12-03 15:59:29
---

<!-- more -->
``` bash
nsupdate -k Kfoo22.bar44.com.+157+12505.private
 #or 
nsupdate -y keyname:secret
```
``` bash command Add CNAME To Zone 
nsupdate
server 192.168.31.50
zone example.com.
update add www.example.com. 86400 CNAME example.com.
show
send
```
``` bash command Delete CNAME From Zone 
nsupdate
server 192.168.31.50
zone example.com.
update delete www.example.com. CNAME
show
send
```

``` bash command nslookup example
 nslookup www.example.com 192.168.31.50
```
