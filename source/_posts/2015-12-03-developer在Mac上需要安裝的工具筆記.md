title: developer在Mac上需要安裝的工具筆記
categories:
  - computer science
  - mac
tags: [mac, python, ios, nodejs, java]
date: 2015-12-03 18:16:57
---
筆記以下目前mac有裝的app
developer取向
<!-- more -->
<!-- toc -->
另外可參考：[mac一般使用安裝軟體筆記](/2015/12/03/mac一般使用安裝軟體筆記/)

## General
### Xcode
``` bash
xcode-select -p
```

### Xcode command line tool
``` bash
xcode-select --install
```

### Homebrew
``` bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
[homebrew指令筆記](/2015/12/03/homebrew指令筆記/)

### macvim
``` bash
brew install macvim
```

### spacemacs
emacs with vim
https://github.com/syl20bnr/spacemacs

### tig
``` bash
brew install tig
```

### dash
API Documents Browser
https://kapeli.com/dash

-------------------------------------------------------------------------------

## IOS、OSX APP開發相關
### CocoaPods
``` bash
sudo gem install cocoapods
```

-------------------------------------------------------------------------------

## Python相關
### pyenv
``` bash
brew install pyenv
```
[pyenv指令筆記](/2015/12/03/pyenv指令筆記/)

### python
``` bash
pyenv install 3.4.2
```

### pyenv-virtualenv
``` bash
brew install pyenv-virtualenv
```
[pyenv-virtualenv指令筆記](/2015/12/03/pyenv-virtualenv指令筆記/)

### use pip to install some libraries or tools about python
``` bash
pip install package_name
```
[pip指令筆記](/2015/12/03/pip指令筆記/)

-------------------------------------------------------------------------------

## Node.js相關
### 裝最新版for global
``` bash
brew install node
```
[npm指令筆記](/2015/12/03/npm指令筆記/)

### nvm(node version manager類似pyenv)
``` bash
brew install nvm
```
[nvm 指令筆記](/2015/12/03/nvm指令筆記/)

-------------------------------------------------------------------------------

## Java相關
### Oracle jdk
http://www.oracle.com/technetwork/articles/javase/index-jsp-138363.html
[Remove jdk 1.8 at Mac OSX yosemite](http://cwza-blog.logdown.com/posts/241790-remove-jdk-18-at-mac-osx-yosemite)

### eclipse
https://www.eclipse.org/downloads/
[Remove eclipse from Mac OSX](http://cwza-blog.logdown.com/posts/241791-remove-eclipse-from-mac-osx)

### pydev for eclipse
http://pydev.org/
