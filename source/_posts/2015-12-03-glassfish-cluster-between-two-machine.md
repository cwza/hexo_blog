title: glassfish cluster between two machine
categories:
  - computer science
  - java
tags: [java, glassfish, linux]
date: 2015-12-03 17:42:19
---

glassfish可以在兩台實體機間做cluster
確保兩台機器上的application共享session......等狀態
兩台機器間會使用ssh做溝通隨時同步狀態，並在其中一台上面有DAS來管理cluster
<!-- more -->
<!-- toc -->

## 環境建置

1. 用virtualbox虛擬兩台linux出來並讓這兩台可以互通
   可參考[VirtualBox guest互聯](/2015/12/03/VirtualBox-guest互聯/)

2. 修改Host Name為n1, n2方便進行測試
   修改方法可參考[Linux更改Host Name](/2015/12/03/Linux更改Host-Name/)

3. 兩台機器上安裝java, glassfish，可參考
   [install glassfish at linux(CentOS)](/2015/12/03/install-glassfish-at-linux-CentOS筆記/) 
   [insall oracle jdk at linux(CentOS)](/2015/12/03/insall-oracle-jdk-at-linux-CentOS筆記/)
   
## 檢查環境
n1上執行:
ssh root@n2 "java -version"
n2上執行:
ssh root@n1 "java -version"

## glassfish設定(使用glassfish admin tool操作:4848)
以下操作均在n1上進行

1. 新增一個遠端n2的node
   Node > New
   Name: n2
   Type: SSH
   SSH user name: root
   SSH user authentication: password
   SSH user password: root password

2. 新增一個cluster
   Clusters > New
   cluster name: c1
   
3. 新增兩個instance一個for n1, 一個for n2
   Server Instances to Be Created > new
   instance name: i1
   Node: localhost-domain1
   Server Instances to Be Created > new
   instance name: i2
   Node: n2
   
4. clusters start cluster

## 佈程式到cluster上
cluster > applications > deploy
