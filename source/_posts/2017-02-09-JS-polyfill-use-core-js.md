title: JS polyfill use core-js
categories: javascript
tags: [javascript]
date: 2017-02-09 17:40:57
---

Create-React-App正常使用下無法更改babel設定
且為了保持產生的bundle.js不會太大
Create-React-App並不會將babel-polyfill全部打開
例如以下這些在IE中將會失敗的Function
Array.includes, String.includes, Object.values
以下簡單介紹3種解決辦法

<!--more-->

## import babel-polyfill
最簡單的方法，但會造成bundle.js增大
``` js
import 'babel-polyfill';
```

## use CDN
一樣會讓user下載整套polyfill
``` html
<script src="https://cdn.polyfill.io/v2/polyfill.min.js"></script>
<script src="https://wzrd.in/standalone/es7-shim@latest"></script>
```

## use core-js
babel-polyfill內部使用的是core-js
所以我們可以直接require core-js即可
當然可以部分require

``` js
export default () => {
  if (!Array.includes) {
    require('core-js/fn/array/includes');
  }
  if (!String.includes) {
    require('core-js/fn/string/includes');
  }
  if (!Object.values) {
    require('core-js/fn/object/values');
  }
  if (!String.endsWith) {
    require('core-js/fn/string/ends-with');
  }
  if (!String.startsWith) {
    require('core-js/fn/string/starts-with');
  }
  if(!Symbol) {
    require('core-js/es6/symbol');
  }
}
```
