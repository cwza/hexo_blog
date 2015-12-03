title: Mapping ip to ip at windows
categories: [computer science, windows]
tags: [windows, ip]
date: 2015-12-03 16:59:06
---

<!-- more -->
## get the interface id of the loop back interface 
``` bash
netsh int ip sh int
```
## add the address to id
``` bash
netsh int ip add addr <ID> <IP>/32 st=ac sk=tr
netsh int ip add addr 1 192.168.2.59/32 st=ac sk=tr
```
## delete the address from id
``` bash
netsh int ip delete addr <ID> <IP>
netsh int ip delete addr 1 192.168.2.59
```
