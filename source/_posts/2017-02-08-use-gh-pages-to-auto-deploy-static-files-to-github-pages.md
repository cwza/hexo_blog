title: use gh-pages to auto deploy static files to github pages
categories: web
tags: [javascript, github, web]
date: 2017-02-08 04:22:57
---

https://github.com/tschaub/gh-pages  
Publish files to a gh-pages branch on GitHub

## install
``` sh
npm install gh-pages --save-dev
```

## config
add following to package.json  
-d : base path of static files
``` js
"scripts": {
  "deploy": "gh-pages -d dist"
}
```

## deploy
``` sh
npm run deploy
```
