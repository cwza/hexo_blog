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

## this
javascript的`this`與一般OO的this很不一樣，他是runtime時才決定指向哪裡的
詳細決定規則請見以下

### Default Binding
當不滿足以下三種狀況時，`this`會自動指向global，strict mode下則是undefined
### Implicit Binding
``` js this指向obj2
function foo() {
    console.log( this.a );
}

var obj2 = {
    a: 42,
    foo: foo
};

var obj1 = {
    a: 2,
    obj2: obj2
};

obj1.obj2.foo(); // 42
```
在callback下，容易發生implicitly lost
``` js Implicitly Lost
function foo() {
    console.log( this.a );
}

var obj = {
    a: 2,
    foo: foo
};

var a = "oops, global"; // `a` also property on global object

setTimeout( obj.foo, 100 ); // "oops, global"
```
### Explicit Binding
可利用call、apply來強制綁定this物件，解決上述的implicitly lost
```js
function foo(something) {
    console.log( this.a, something );
    return this.a + something;
}

// simple `bind` helper
// bind obj to this of fn
function bind(fn, obj) {
    return function() {
        return fn.apply( obj, arguments );
    };
}

var obj = {
    a: 2
};

var bar = bind( foo, obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```
ES5內建bind方法可簡單達成上述
```js
function foo(something) {
    console.log( this.a, something );
    return this.a + something;
}

var obj = {
    a: 2
};

var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```
### New Binding
``` js
function foo(a) {
    this.a = a;
}

var bar = new foo( 2 ); // this is binding to bar
console.log( bar.a ); // 2
```

### So How To Determine This
1. Is the function called with `new` (**new binding**)? If so, `this` is the newly constructed object.

    `var bar = new foo()`

2. Is the function called with `call` or `apply` (**explicit binding**), even hidden inside a `bind` *hard binding*? If so, `this` is the explicitly specified object.

    `var bar = foo.call( obj2 )`

3. Is the function called with a context (**implicit binding**), otherwise known as an owning or containing object? If so, `this` is *that* context object.

    `var bar = obj1.foo()`

4. Otherwise, default the `this` (**default binding**). If in `strict mode`, pick `undefined`, otherwise pick the `global` object.

    `var bar = foo()`  

### Safer this
如果你發現你需要apply或bind的parameter spread、currying功能
但卻不care this到底是什麼，你可能會想用apply(null, [2, 3])
這可能導致this指向global(default binding)
底下是一種比較好的方式，create一個empty object來取代null
``` js
function foo(a,b) {
    console.log( "a:" + a + ", b:" + b );
}

// our DMZ empty object
var ø = Object.create( null );

// spreading out array as parameters
foo.apply( ø, [2, 3] ); // a:2, b:3

// currying with `bind(..)`
var bar = foo.bind( ø, 2 );
bar( 3 ); // a:2, b:3
```

### arrow function
ES6新增的arrow function，有著跟一般function不一樣的this行為
其this永遠指向包圍它的function的this，使其在callback中很有用
``` js
function foo() {
    setTimeout(() => {
        // `this` here is lexically adopted from `foo()`
        console.log( this.a );
    },100);
}

var obj = {
    a: 2
};

foo.call( obj ); // 2
```

然而在不用arrow function時就已經有好解法了：
``` js
function foo() {
    var self = this; // lexical capture of `this`
    setTimeout( function(){
        console.log( self.a );
    }, 100 );
}

var obj = {
    a: 2
};

foo.call( obj ); // 2
```
當然你也能用bind來處理callback的this，這三種方法就是程式風格的不同而已，儘量統一就好

## ES6新東東
let, const, arrow function, class, module

