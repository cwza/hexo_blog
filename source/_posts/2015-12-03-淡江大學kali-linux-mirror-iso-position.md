title: '淡江大學kali-linux mirror & iso position'
categories: linux
tags: [linux, kali]
date: 2015-12-03 16:24:47
---

<!-- more -->

官方的 Source 在台北連線實在很慢，，淡江大學資訊中心的伺服器有 Mirror 一份，直接改 /etc/apt/sources.list 替換成下面這兩個位址就可以了。
``` text mirror site
http://ftp.tku.edu.tw/kali/ main non-free contrib
http://ftp.tku.edu.tw/kali-security/ kali/updates main contrib non-free
```
``` text iso
http://ftp.tku.edu.tw/index.php?dir=Linux%2FKali%2F
```
