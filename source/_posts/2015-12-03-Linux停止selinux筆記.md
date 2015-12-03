title: Linux停止selinux筆記
categories: [computer science, linux]
tags: [linux]
date: 2015-12-03 17:24:15
---
這篇是轉貼
參考以下網站
http://bojack.pixnet.net/blog/post/4039227-【centos】關閉-selinux-~
<!-- more -->

SELinux 是 Security Enhanced Linux (安全加強的 Linux) 的縮寫， 他並不是一個防火牆的軟體，而是一個『針對檔案系統權限作更細部規劃的一個模組』。

 傳統的 Linux 權限是分為三種身份 (owner, group, others) 以及三種權限 (r, w, x)， 但事實上，這三種身份的三種權限組合並無法有效的管理所有系統上的 daemon 存取資料時所需要的行為。 因此美國國家安全局便發展出這個可以更細部規劃檔案權限功能的 SELinux 了。

由於 SELinux 主要是進行檔案系統的細部權限設定，所以想要使用 SELinux 的配置時， 需要對 Linux 的檔案系統以及基礎的作業系統概念要很清楚，否則將會使得很多的網路服務無法正確的啟用系統資源， 導致你的主機很多服務無法存取系統資料！因此，對於我們剛接觸到 Linux 架站的朋友來說， 建議你先關閉 SELinux ，等到兩三年後對於 Linux 有很深的概念後， 再來嘗試配置 SELinux 這個有趣的咚咚！

也就是說，如果你沒有關閉 SELinux 的話，那麼你就得要針對 SELinux 進行檔案權限的額外配置， 否則你的網路服務就不可能會正常的啟動！
要關掉 SELinux 這東東很簡單，去編輯 /etc/sysconfig/selinux 這個檔案

將 SELINUX=enforcing 改成 SELINUX=disabled ，重開機就可以了！



setenforce 0可以馬上關掉SELinux
