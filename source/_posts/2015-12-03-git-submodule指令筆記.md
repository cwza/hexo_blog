title: git submodule指令筆記
categories:
  - computer science
  - git
tags: [git]
date: 2015-12-03 18:49:41
---

<!-- more -->

## add submodule
``` bash
git submodule add http://github.com/tpope/vim-fugitive.git bundle/fugitive
git add .
git commit -m "Install Fugitive.vim bundle as a submodule."
```

## remove submodule
1. Delete the relevant section from the **.gitmodules** file.
2. Stage the **.gitmodules** changes **git add .gitmodules**
3. Delete the relevant section from **.git/config**.
4. Run **git rm --cached path_to_submodule** (no trailing slash).
5. Run **rm -rf .git/modules/path_to_submodule**
6. commit **git commit -m "Removed (submodule name)"**
7. Delete the now untracked submodule files **rm -rf path_to_submodule**
8. git submodule sync

## clone a project with git submodule
``` bash
git clone <project>
git submodule init
git submodule update
```
