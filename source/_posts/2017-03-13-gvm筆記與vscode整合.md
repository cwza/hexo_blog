title: gvm筆記與vscode整合
categories: golang
tags: [golang]
date: 2017-03-13 18:11:57
---

## 安裝
``` bash
xcode-select --install
brew update
brew install mercurial
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
gvm install go1.4 -B
gvm use go1.4
export GOROOT_BOOTSTRAP=$GOROOT
gvm install 1.8
```

## 指令
``` bash
gvm install [version]
gvm uninstall [version]
gvm listall
gvm list
gvm use [version]
gvm implode    // uninstall gvm
```

## 使用gvm來管理workspace
``` bash
go use [version]
mkdir -p ~/golang/  
cd ~/golang/  
gvm pkgset create --local  
gvm pkgset use --local  
mkdir src pkg bin
go env
```
利用pkgset將GOPATH設為自定的workspace path

## vscode 整合
設定gopath與goroot為go env的值
``` bash
go use [version]
go env
```
``` json
{
  "go.gopath":"/Users/meep007/cwz/develop/practice/golang:/Users/meep007/cwz/develop/practice/golang/.gvm_local/pkgsets/go1.8/local:/Users/meep007/.gvm/pkgsets/go1.8/global",
  "go.goroot": "/Users/meep007/.gvm/gos/go1.8"
}
```