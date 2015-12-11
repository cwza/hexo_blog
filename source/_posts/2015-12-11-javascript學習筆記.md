title: javascript學習筆記
categories:
  - computer science
  - javascript
tags: [javascript]
date: 2015-12-11 14:30:50
---
隨筆亂記
陸續增加中
<!-- more -->
<!-- toc -->

`The function of javascript is just an Object so it can has properties or functions in it.`

## javascript built-in types
* `string`
* `number`
* `boolean`
* `null` and `undefined`
* `object`
* `symbol` (new to ES6)

## Hoisting
可以想像成所有宣告時機(var a)都被提升到function最開始，更精確的說是scope的最開始
實際上是javascript interpreter將var a = 2分成兩個步驟
declaration(var a)，assignment(a = 2)
所有declaration在compile time完成，assignment在runtime執行
須注意的是ES6新增的block scope variable `let & const`，沒有hoisting機制
當然eval("var a = 2;");這種用法也會破壞hoisting，不過為了效能本來就該避免eval這東西了
``` js
var a = 2;

foo();					// works because `foo()`
						// declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
}

console.log( a );	// 2
```

## Closure
> Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.

foo()執行完後，local variable a沒被回收的原因是因為baz -> bar還擁有reference to foo，所以a不會被GC回收 
``` js
function foo() {
    var a = 2;

    function bar() {
        console.log( a );
    }

    return bar;
}

var baz = foo();

baz(); // 2 -- Whoa, closure was just observed, man.
```
``` js some useful snippet
function wait(message) {

    setTimeout( function timer(){
        console.log( message );
    }, 1000 );

}

wait( "Hello, closure!" );
```

## Immediately Invoked Function Expressions (IIFEs)
```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

## Modules
``` js
var foo = (function CoolModule(id) {
    function change() {
        // modifying the public API
        publicAPI.identify = identify2;
    }

    function identify1() {
        console.log( id );
    }

    function identify2() {
        console.log( id.toUpperCase() );
    }

    var publicAPI = {
        change: change,
        identify: identify1
    };

    return publicAPI;
})( "foo module" );

foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```
### Modern Modules
```js module dependency loaders/managers
var MyModules = (function Manager() {
    var modules = {};

    function define(name, deps, impl) {
        for (var i=0; i<deps.length; i++) {
            deps[i] = modules[deps[i]];
        }
        modules[name] = impl.apply( impl, deps );
    }

    function get(name) {
        return modules[name];
    }

    return {
        define: define,
        get: get
    };
})();
```
```js use above
MyModules.define( "bar", [], function(){
    function hello(who) {
        return "Let me introduce: " + who;
    }

    return {
        hello: hello
    };
} );

MyModules.define( "foo", ["bar"], function(bar){
    var hungry = "hippo";

    function awesome() {
        console.log( bar.hello( hungry ).toUpperCase() );
    }

    return {
        awesome: awesome
    };
} );

var bar = MyModules.get( "bar" );
var foo = MyModules.get( "foo" );

console.log(
    bar.hello( "hippo" )
); // Let me introduce: hippo

foo.awesome(); // LET ME INTRODUCE: HIPPO
```
### Future Modules
ES6新語法，支援File based Modules
```js bar.js
function hello(who) {
    return "Let me introduce: " + who;
}

export hello;
```
```js foo.js
// import only `hello()` from the "bar" module
import hello from "bar";

var hungry = "hippo";

function awesome() {
    console.log(
        hello( hungry ).toUpperCase()
    );
}

export awesome;
```
```js use above
// import the entire "foo" and "bar" modules
module foo from "foo";
module bar from "bar";

console.log(
    bar.hello( "rhino" )
); // Let me introduce: rhino

foo.awesome(); // LET ME INTRODUCE: HIPPO
```
