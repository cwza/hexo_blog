title: Red hat底下安裝glassfish並註冊成為service
categories: java
tags: [java, linux, glassfish]
date: 2015-12-03 16:39:56
---
前兩步是要用oracle jdk取代open jdk，glassfish安裝從第3步開始
<!-- more -->

<!-- toc --> 

## 移除open jdk所有版本
``` bash
 #delete openjdk and all dependancies, we do not need these
yum remove java*
```

## 安裝oracle jdk
1. 下載jdk1.7
     http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html
     Java SE Development Kit 7u55
     Linux x64  jdk-7u55-linux-x64.tar.gz
2. 解壓縮到/usr/java/
``` bash
tar -zxvf jdk1.7.0_55.tar.gz
```
3. 修改/etc/profile與/etc/bashrc，加入以下
``` bash
    # JDK environment
    JAVA_HOME=/usr/java/jdk1.7.0_55
    PATH=$PATH:$JAVA_HOME/bin/
```
4. 登出再登入，or source /etc/profile
5. 檢查
``` bash
echo $PATH
java -version
 #結果:java version "1.7.0_55"
```

## 安裝glassfish
1. 下載glassfish4.0
    http://download.java.net/glassfish/4.0/release/glassfish-4.0.zip
2. 解壓縮到/usr/glassfish
``` bash
unzip glassfish4.zip
```
3. 開啟glassfish server
``` bash
cd /usr/glassfish/glassfish4/glassfish/bin
./asadmin start-domain domain1
```


## glassfish asadmin指令(包含domain的啟動關閉，check status指令)
* use asadmin to start glassfish domain1:
``` bash
cd /usr/glassfish/glassfish4/glassfish/bin
./asadmin start-domain domain1
```
* use asadmin to stop glassfish domain1:
``` bash
cd /usr/glassfish/glassfish4/glassfish/bin
./asadmin stop-domain domain1
```
* use asadmin to check glassfish domain status:
``` bash
./asadmin list-domains
 #結果: domain1 running
```
* use ps to check glassfish:
``` bash
ps -ef | grep glassfish
```
* glassfish admin tool page:
``` bash
http://localhost:4848
```

## 註冊glassfish service 
1. ./asadmin create-service
   會在/etc/init.d裡多一個shell file: GlassFish_domain1
   /etc/rcX.d裡多soft link S20GlassFish_domain1 or K20GlassFish_domain1
2. 修改/etc/init.d/GlassFish_domain1
   最上面加入source /etc/profile
