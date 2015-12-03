title: install glassfish at linux CentOS筆記
categories: [computer science, java]
tags: [glassfish, linux, java]
date: 2015-12-03 16:14:53
---

<!-- more -->
要先裝java
可參考[insall oracle jdk at linux(CentOS)](/2015/12/03/insall-oracle-jdk-at-linux-CentOS筆記/)

1. 下載glassfish
2. 解壓縮到/usr/glassfish
``` bash
unzip glassfish4.zip
```

3. 開啟glassfish server
``` bash
cd /usr/local/glassfish4/glassfish/bin
./asadmin start-domain domain1
```

4. 
  * 管理介面 http://localhost:4848
  * http://localhost:8080
