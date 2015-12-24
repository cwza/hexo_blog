title: windows修改route筆記
categories:
  - windows
tags: [windows]
date: 2015-12-03 17:54:48
---


## Route print 用來顯示路由表

| destination | netmask   |  Gateway    | interface    | matric |
| ----------- | --------- | ----------- | ------------ | ------ |
| 0.0.0.0     | 0.0.0.0   | 192.168.0.1 | 192.168.0.40 | 25     |
| 127.0.0.0   | 255.0.0.0 | 在連結上     | 127.0.0.1    | 306    |
<!-- more -->

第一筆（Network Destination：0.0.0.0 .... ）是預設路徑（default route），只要路由表找不到傳送路徑的封包，最後都會由會交由預設路徑傳送，因為不論是什麼 IP 位址與 0.0.0.0 的網路遮罩作 AND 運算，結果都是 0.0.0.0，因此封包會被傳送到 192.168.1.1 此一Gateway。

第二筆（Network Destination：127.0.0.1 ... ）是迴路路徑，因此所有要傳送到 127.x.x.x 的封包最候都會送到 127.0.0.1 IP 位址，也就是電腦自己。
        
## route add 用來加入路由路徑
例如：route add 192.168.0.0 mask 255.255.0.0 192.168.1.1 if 0x2 metric 20
指出 Network Destination、Netmask、Gateway、Interface 和 metric。


## route -p add 用來永久加入路由路徑，使用-p 參數可以保留路徑設定，不會因為電腦重開機而消失。
例如：route -p add 192.168.0.0 mask 255.255.0.0 192.168.1.1 if 0x2 metric 20。


## route delete用來刪除路由路徑。
例如：route delete 192.168.0.0 mask 255.255.0.0。


## route change用來修改現有的路徑設定。
例如：route change 192.168.0.0 mask 255.255.0.0 192.168.1.1 if 0x2 metric 10
