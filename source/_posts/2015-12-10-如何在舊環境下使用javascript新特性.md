title: 如何在舊環境下使用javascript新特性
categories:
  - computer science
  - javascript
tags: [javascript, nodejs]
date: 2015-12-10 15:27:09
---
javascript是近年來進步非常快的語言
Browser常常趕不上ECMA標準
導致ES6有非常多好用的新功能目前主流Browser都還沒完全支援
即使如此還是建議大家儘量使用ES6的新東西來寫
用以下提到的Polyfilling、Transpiling來做

[主流Browser對ES6支援狀況](https://kangax.github.io/compat-table/es6/)

<!-- more -->

## Polyfilling
舉個ES6新增的Number.isNan來說明：
``` js
if (!Number.isNaN) {  //檢查環境是否支援
    Number.isNaN = function isNaN(x) { //不支援就定義一個
        return x !== x;
    };
}
```
實務上當然不用自己寫
以下是別人寫好現成的
ES5-Shim (https://github.com/es-shims/es5-shim) and ES6-Shim (https://github.com/es-shims/es6-shim)


## Transpiling
若有新語法
則上述方法就無法使用了
此時需要使用一些工具將source轉換為舊版相容的code
例如ES6的default parameter values:
``` js
function foo(a = 2) {
    console.log( a );
}

foo();      // 2
foo( 42 );  // 42
```

舊語法:
``` js
function foo() {
    var a = arguments[0] !== (void 0) ? arguments[0] : 2;
    console.log( a );
}
```

以下是兩種可用的轉換工具
* Babel (https://babeljs.io) (formerly 6to5): Transpiles ES6+ into ES5
* Traceur (https://github.com/google/traceur-compiler): Transpiles ES6, ES7, and beyond into ES5
