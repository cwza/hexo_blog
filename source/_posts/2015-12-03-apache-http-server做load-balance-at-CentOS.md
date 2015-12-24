title: apache http server做load balance at CentOS
categories:
  - apache
tags: [linux, apache, glassfish]
date: 2015-12-03 17:27:59
---


## 在兩台虛擬機上各裝一個glassfish與測試application
事先放一個測試servlet /test/hello在n1, n2兩台機器上
裝glassfish要先裝java jdk並設環境變數
  

## 需要程式:
<!-- more -->
httpd, mod_proxy, mod_ssl, mod_proxy_balancer

1. 安裝httpd
```
yum install httpd 
或http://www.rpmfind.net/找rpm
安裝完後會在下列路徑找到/etc/httpd
log在/var/log/httpd/error_log
```

2. 到/etc/httpd/modules找找有無mod_proxy, mod_ssl
   沒有的話去http://www.rpmfind.net/找rpm安裝
   注意裝的版本要跟httpd完全一致
CentOS中通常只會缺mod_ssl

3. 停止 SELinux
   編輯 /etc/sysconfig/selinux 這個檔案
   將 SELINUX=enforcing 改成 SELINUX=disabled
   執行setenforce 0

4. 設定mod_proxy
   /etc/httpd/conf.d/中加入mod_proxy.conf
   其中n1是machine1的domain, n2是machine2的domain
``` apache
Listen 9090
Listen 9191
NameVirtualHost *:9090
NameVirtualHost *:9191
<VirtualHost *:9090>
        ProxyRequests off

        ServerName n1
        <Proxy balancer://mycluster>
                # WebHead1
                BalancerMember http://n1:8080
                # WebHead2
                BalancerMember http://n2:8080

                # Security "technically we aren't blocking
                # anyone but this the place to make those
                # chages
                Order Deny,Allow
                Deny from none
                Allow from all

                # Load Balancer Settings
                # We will be configuring a simple Round
                # Robin style load balancer.  This means
                # that all webheads take an equal share of
                # of the load.
                ProxySet lbmethod=byrequests

        </Proxy>

        # balancer-manager
        # This tool is built into the mod_proxy_balancer
        # module and will allow you to do some simple
        # modifications to the balanced group via a gui
        # web interface.
        <Location /balancer-manager>
                SetHandler balancer-manager

                # I recommend locking this one down to your
                # your office
                Order deny,allow
                Allow from all
        </Location>

        # Point of Balance
        # This setting will allow to explicitly name the
        # the location in the site that we want to be
        # balanced, in this example we will balance "/"
        # or everything in the site.
        ProxyPass /balancer-manager !
        ProxyPass / balancer://mycluster/

</VirtualHost>
<VirtualHost *:9191>
        ProxyRequests off
        SSLEngine on
        ServerName n1
        SSLCertificateFile /etc/pki/tls/certs/localhost.crt
        SSLCertificateKeyFile /etc/pki/tls/private/localhost.key

        <Proxy balancer://mycluster>
                # WebHead1
                BalancerMember http://n1:8080
                # WebHead2
                BalancerMember http://n2:8080

                # Security "technically we aren't blocking
                # anyone but this the place to make those
                # chages
                Order Deny,Allow
                Deny from none
                Allow from all

                # Load Balancer Settings
                # We will be configuring a simple Round
                # Robin style load balancer.  This means
                # that all webheads take an equal share of
                # of the load.
                ProxySet lbmethod=byrequests

        </Proxy>

        # balancer-manager
        # This tool is built into the mod_proxy_balancer
        # module and will allow you to do some simple
        # modifications to the balanced group via a gui
        # web interface.
        <Location /balancer-manager>
                SetHandler balancer-manager
                # I recommend locking this one down to your
                # your office
                Order deny,allow
                Allow from all
        </Location>

        # Point of Balance
        # This setting will allow to explicitly name the
        # the location in the site that we want to be
        # balanced, in this example we will balance "/"
        # or everything in the site.
        ProxyPass /balancer-manager !
        ProxyPass / balancer://mycluster/

</VirtualHost>
```
5. 關閉iptables並啟動httpd
   service stop iptalbes
   service start httpd

6. 將httpd設定為開機即啟動，iptables設定成不要開機啟動
   chkconfig --levels 235 httpd on
   chkconfig iptables off
   
7. http://localhost:9090/balancer-manager
   apache proxy_balancer簡單管理頁面

8. http://n1:9090/test/hello並觀察balancer-manager頁面

9. 可以將/etc/httpd/conf/httpd.conf的KeepAlive改為On
