title: git config筆記
categories:
  - computer science
  - git
tags: [git]
date: 2015-12-03 19:19:39
---

<!-- more -->
## git config三層設定
```
git config --system
針對所有使用者
/etc/gitconfig

git config --global
針對特定user
~/.gitconfig

git config
針對特定專案
.git/config
```

``` bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
#防止中文亂碼
git config --global core.quotepath false
git config --global core.editor emacs
git config --global commit.template $HOME/.gitmessage.txt
git config --global color.ui true
```
