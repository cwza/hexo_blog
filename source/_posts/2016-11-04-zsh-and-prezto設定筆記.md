title: zsh and prezto設定筆記
categories: mac
tags: [mac, linux, zsh]
date: 2016-11-02 17:17:57
---
<!-- toc -->

Zsh + Prezto可實現更方便好用的Terminal
本文描述在Mac上安裝Zsh設定文件Prezto的方法

Mac上建議可搭配iTerm2使用

<!--more-->

# 安裝
## Launch Zsh
``` bash
zsh
```
##  get the code from github
``` bash
git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"
```
## add ~/.zshrc and source init.zsh to it
``` bash
# ~/.zshrc
# Source Prezto.
if [[ -s "${ZDOTDIR:-$HOME}/.zprezto/init.zsh" ]]; then
  source "${ZDOTDIR:-$HOME}/.zprezto/init.zsh"
fi
```
## 設定預設terminal為zsh
``` bash
chsh -s /bin/zsh
```
## install Powerline fonts
如果要使用powerline系列主題的話，需要安裝powerline系列字型
``` bash
git clone https://github.com/powerline/fonts.git
cd fonts
./install.sh
```
安裝完後在terminal的字型中選用有powerline的字型


# 設定
## add ~/.zpreztorc and enable modules
prompt -l可列出主題，prompt {主題名稱}可套用主題到現有session上
以下設定開啟的主題為agnoster
支援的module列表可參照https://github.com/sorin-ionescu/prezto/tree/master/modules
``` bash
#~/.zpreztorc
# Color output (auto set to 'no' on dumb terminals).
zstyle ':prezto:*:*' color 'yes'

# Set the Prezto modules to load (browse modules).
# The order matters.
zstyle ':prezto:load' pmodule \
   'directory' \
   'utility' \
   'completion' \
   'git' \
   'syntax-highlighting' \
   'history-substring-search' \
   'autosuggestions' \
   'prompt' \

# Set the prompt theme.
zstyle ':prezto:module:prompt' theme 'agnoster'
```

## Remove category menu of tab complete
``` sh
# ~/.zprezto/modules/completion/init.zsh
...
# Group matches and describe.
zstyle ':completion:*:*:*:*:*' menu select
# zstyle ':completion:*:matches' group 'yes'
# zstyle ':completion:*:options' description 'yes'
# zstyle ':completion:*:options' auto-description '%d'
# zstyle ':completion:*:corrections' format ' %F{green}-- %d (errors: %e) --%f'
# zstyle ':completion:*:descriptions' format ' %F{yellow}-- %d --%f'
# zstyle ':completion:*:messages' format ' %F{purple} -- %d --%f'
# zstyle ':completion:*:warnings' format ' %F{red}-- no matches found --%f'
# zstyle ':completion:*:default' list-prompt '%S%M matches%s'
# zstyle ':completion:*' format ' %F{yellow}-- %d --%f'
# zstyle ':completion:*' group-name ''
# zstyle ':completion:*' verbose yes
...
```

# Update Prezto
``` bash
cd ~/.zprezto
git pull && git submodule update --init --recursive
```
