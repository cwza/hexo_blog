title: Bind9的Dynamic Dns Update設定筆記
date: 2015-12-02 19:27:30
categories: linux
tags: [linux, bind9]
---
## Something
bind9支援dynamic dns update request，可以動態的在remote送出dns update去更改dns server zone的設定
bind9在接收到dns update request後會先在/etc/bind/底下新增db-abc.com.tw.jnl，以之達成動態修改db-abc.com.tw文件，db-abc.com.tw文件會在下次server restart時才真正寫入
當然在zone中要設定allow-update，預設是不允許的

<!-- more -->

## Bind9 allow-update 設定
1. 產生MD5 Key
	 dnssec-keygen -a HMAC-MD5 -b 128 -n HOST example.com.
2. cat example.com.+157+00000.key
   example.com. IN KEY 512 3 157 +Cdjlkef9ZTSeixERZ433Q==
3. 在named.conf.local中加入allow-update
``` zone named.conf.local
zone "abc.com.tw" IN {
     type master;
     file "/etc/bind/db-abc.com.tw";
     allow-update { key example.com.; };
};
key example.com. { 
    algorithm hmac-md5; 
    secret "+Cdjlkef9ZTSeixERZ433Q=="; 
};
```
4. service bind9 restart
5. 記得將/etc/bind權限設給bind，否則在dns update時會有permission denied error
   chmod 777 /etc/bind
   chown bind:bind /etc/bind
   
## Refefence 
[Setting up secure updates using TSIG keys for BIND 9](https://sort.symantec.com/public/documents/sfha/6.0/linux/productguides/html/vcs_bundled_agents/ch03s06s06s06.htm)
