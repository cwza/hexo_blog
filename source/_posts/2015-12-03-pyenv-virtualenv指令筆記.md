title: pyenv-virtualenv指令筆記
categories:
  - computer science
  - python
tags: [python]
date: 2015-12-03 18:35:46
---

<!-- more -->
## Mac下安裝
``` bash
brew install pyenv-virtualenv
```

## 指令
``` bash
#新增一個以python3.4為底的virtual env在/usr/local/opt/pyenv/versions
pyenv virtualenv 3.4 my-virtual-env-3.4
#移除
pyenv uninstall my-virtual-env﹣3.4
```
其他請參照[pyenv指令筆記](/2015/12/03/pyenv指令筆記/)

