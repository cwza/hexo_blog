title: 在Mac finder中顯示隱藏檔案資料夾
categories:
  - mac
tags: [mac]
date: 2015-12-03 18:04:21
---

<!-- more -->
在finder中顯示隱藏檔案
``` bash
defaults write com.apple.finder AppleShowAllFiles TRUE && killall Finder
```
恢復隱藏
``` bash
defaults write com.apple.finder AppleShowAllFiles FALSE && killall Finder
```
