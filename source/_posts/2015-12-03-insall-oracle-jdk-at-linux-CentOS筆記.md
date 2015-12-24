title: insall oracle jdk at linux CentOS筆記
categories: java
tags: [java, linux]
date: 2015-12-03 16:12:18
---

<!-- more -->
1. 下載jdk
2. 解壓縮到/usr/java/
```
tar -zxvf jdk1.7.0_45.tar.gz
```

3. 修改/etc/profile與/etc/profile/etc/bashrc，加入以下
``` bash
# JDK environment
JAVA_HOME=/usr/java/jdk1.7.0_45
PATH=$PATH:$JAVA_HOME/bin/
```

4. 登出再登入，or source /etc/profile
5. java -version

