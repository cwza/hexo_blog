title: My vim settings at Github
categories:
  - computer science
  - editor
tags: [vim, editor]
date: 2015-12-03 18:46:42
---
轉移到spacemacs所以這篇大概也沒用了
不過設定檔都還放在github
要用也行
<!-- more -->

My vimrc is fork from https://github.com/vgod/vimrc

1. Check out from github
``` bash
git clone https://github.com/cwza/vimrc.git ~/.vim
cd ~/.vim
git submodule update --init
```
2. Install ~/.vimrc and ~/.gvimrc(do not neet to do this for mvim)
``` bash
./install-vimrc.sh
```
3. (Optional, if you want Command-T) Compile the Command-T plugin
``` bash 
cd .vim/bundle/command-t/ruby/command-t
ruby extconf.rb
make
```
