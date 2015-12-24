title: Linux更改Host Name
categories: linux
tags: [linux]
date: 2015-12-03 17:22:18
---

<!-- more -->
ex: 將aaa改為n1

1. 修改 /etc/hosts
```
127.0.0.1 aaa
=>
127.0.0.1 n1
```

2. 修改/etc/sysconfig/network
```
NETWORKING=yes
HOSTNAME=n1
```

3. reboot

4. hostname
