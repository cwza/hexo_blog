title: Digital Ocean Domain And Swap Setting
categories: web
tags: [web, digital-ocean]
date: 2017-02-09 16:21:57
---

## Domain Setting
1. 買一個domain(namecheap之類的)
2. 照著官方文件做吧
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-host-name-with-digitalocean

## Swap Setting
Digital Ocean每月5美金的那個VPS，記憶體只有512MB
並且Digital Ocean預設OS的Swap是關閉的（官方說明SSD不建議使用swap）
若在上面再用docker的話高機率因為記憶體不足process被kill掉
若是真的不想加錢買更多記憶體的話還是可以手動開啟swap

一樣官方文件照著做就好
https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04
