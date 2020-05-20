---
title: JavaScript模块化历程
date: 2020-05-19 21:34:01
categories: JavaScript
tags:
	- JavaScript模块化
---



# 前言

到目前为止，JavaScript模块化大概分为以下几个里程碑式节点。

* 函数块

* 命名空间

* 闭包

* CommonJS

* AMD

* CMD

* UMD

* ES6Module

  

<!-- more -->

# 函数块

最早，我们这么写代码

```
function foo() {}
function bar() {}
```

{% note warning %}
Global 被污染，很容易命名冲突
{% endnote %}



# 命名空间

```
var MYAPP = {
    foo: function(){},
    bar: function(){}
}

MYAPP.foo();
```

{% note warning %}

* 减少 Global 上的变量数目

* 本质是对象，一点都不安全

{% endnote %}



# 闭包（IIFE 模式）

## 匿名闭包 

```
var Module = (function(){
    var _private = "safe now";
    var foo = function(){
        console.log(_private)
    }

    return {
        foo: foo
    }
})()

Module.foo();
Module._private; // undefined
```

{% note warning %}
函数是 JavaScript 唯一的 Local Scope
{% endnote %}



## 引入依赖

```
var Module = (function($){
    var _$body = $("body");     // we can use jQuery now!
    var foo = function(){
        console.log(_$body);    // 特权方法
    }

    // Revelation Pattern
    return {
        foo: foo
    }
})(jQuery)

Module.foo();
```

{% note warning %}
这就是模块模式，也是现代模块实现的基石
{% endnote %}



# [CommonJS](http://www.commonjs.org/)&& [Node.js](https://nodejs.org/en/)(2009) 

`CommonJS`规范，主要运行于`服务器端`，一个单独的文件就是一个模块，`同步加载`模块。`Node.js`很好的实现了该规范。

```
// module1.js
exports.add = function(a, b){
    return a + b;
}
// main.js
var module1 = require('./module1')	// 同步加载并执行
console.log(module1.add(1, 2));    	// 3
```

{% note warning %}

同步/阻塞式加载

{% endnote %}


![image-20200520112846849](https://yuanchangjian.github.io/cloudImage/images/20200519222015.png)



# [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD) && [Require.js](https://requirejs.org/)(2011)

AMD(Asynchronous Module Definition - 异步加载模块定义)规范，一个单独的文件就是一个模块，模块和模块的依赖可以被`异步加载`。主要运行于`浏览器端`。该规范是在RequireJs的推广过程中逐渐完善的。

```
// moduleA.js
define(
	['jQuery','lodash'], 	// 依赖
	function($, _) {	// 这个回调会在所有依赖都被加载后才执行
        var name = 'weiqinl',
        function foo() {}
        return {
            name,
            foo
        }
    }
)

// index.js
require(['moduleA'], function(a) {
    a.name === 'weiqinl' // true
    a.foo() // 执行A模块中的foo函数
    // ...
})

// index.html
<script src="js/require.js" data-main="js/index"></script>
```

## AMD vs CommonJS

* 书写风格

```
// Module/1.0
var a = require("./a");  // 依赖就近
a.doSomething();

var b = require("./b")
b.doSomething();
```

```
// AMD recommended style
define(["a", "b"], function(a, b){ // 依赖前置
    a.doSomething();
    b.doSomething();
})
```

* 执行时机

```
// Module/1.0
var a = require("./a");  // 执行到此时，a.js 同步下载并执行
```

```
// AMD with CommonJS sugar
define(["require"], function(require){
    // 在这里， a.js 已经下载并且执行好了
    var a = require("./a")
})
```

AMD的运行逻辑是：提前加载，提前执行。在Requirejs中，申明依赖模块时，会第一时间加载并执行模块内的代码，使后面的回调函数能在所需的环境中运行。



# [CMD](https://github.com/cmdjs/specification/blob/master/draft/module.md) && Sea.js(2011)

CMD(Common Module Definition - 通用模块定义)规范主要是Sea.js推广中形成的，一个文件就是一个模块，可以像Node.js一般书写模块代码。主要在浏览器中运行，当然也可以在Node.js中运行。

```
// moduleA.js
// 定义模块
define(function(require, exports, module) {
    var func = function() {
        var a = require('./a') // 到此才会加载a模块
        a.func()
        
        if(false) {
        	var b = require('./b') // 到此才会加载b模块，此处条件不法进入，不会加载
        	b.func() 	
    	}
	}
	
	// ...
	
	exports.func = func;
})

// index.js
// 加载使用模块
seajs.use('moduleA.js', function(ma) {
	var ma = math.func()
})

// HTML，需要在页面中引入sea.js文件。
<script src="./js/sea.js"></script>
<script src="./js/index.js"></script>
```



## AMD VS CMD

* 执行时机

```
// AMD recommended
define(['a', 'b'], function(a, b){
    a.doSomething();    // 依赖前置，提前执行
    b.doSomething();
})
```

```
// CMD recommanded
define(function(require, exports, module){
    var a = require("a");
    a.doSomething();
    var b = require("b");
    b.doSomething();    // 依赖就近，延迟执行
})
```

CMD推崇依赖就近，延迟执行。在上面例子中，通过require引入的模块，只有当程序运行到此处的时候，模块才会自动加载执行。


# UMD && webpack

UMD(Universal Module Definition - 通用模块定义)模式，该模式主要用来解决CommonJS模式和AMD模式代码不能通用的问题，并同时还支持老式的全局变量规范。

```
// 使用Node, AMD 或 browser globals 模式创建模块
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
    	// AMD模式. 注册为一个匿名函数
		define(['b'], factory);
    } else if (typeof module === 'object' && module.exports) {
    	// Node等类CommonJS的环境
		module.exports = factory(require('b'));
    } else {
    	// 浏览器全局变量 (root is window)
		root.returnExports = factory(root.b);
    }
}(typeof self !== 'undefined' ? self : this, function (b) {
	// 以某种方式使用 b
    //返回一个值来定义模块导出。(即可以返回对象，也可以返回函数)
    return {};
}));
```

1. 判断`define`为函数，并且是否存在`define.amd`，来判断是否为`AMD规范`
2. 判断`module`是否为一个对象，并且是否存在`module.exports`来判断是否为`CommonJS规范`
3. 如果以上两种都没有，设定为原始的代码规范

这种模式，通常会在`webpack`打包的时候用到。`output.libraryTarget`将模块以哪种规范的文件输出。



# ES6 Module && ES6

在ECMAScript 2015版本出来之后，确定了一种新的模块加载方式，我们称之为ES6 Module。它和前几种方式有区别和相同点。

1. 它因为是标准，所以未来很多浏览器会支持，可以很方便的在浏览器中使用。
2. 它同时兼容在`node`环境下运行。
3. 模块的导入导出，通过`import`和`export`来确定。
4. 可以和`Commonjs`模块混合使用。
5. `CommonJS`输出的是一个**值的拷贝**。ES6模块输出的是**值的引用**,加载的时候会做静态优化。
6. `CommonJS`模块是运行时加载确定输出接口，ES6模块是编译时确定输出接口。

ES6模块功能主要由两个命令构成：`import`和`export`。`import`命令用于输入其他模块提供的功能。`export`命令用于规范模块的对外接口。

```
// 输出变量
export var name = 'weiqinl'
export var year = '2018'

// 输出一个对象（推荐）
var name = 'weiqinl'
var year = '2018'
export { name, year}


// 输出函数或类
export function add(a, b) {
	return a + b;
}

// export default 命令
export default function() {
	console.log('foo')
}
```

import导入其他模块

```
// 正常命令
import { name, year } from './module.js' //后缀.js不能省略

// 如果遇到export default命令导出的模块
import ed from './export-default.js'
```



## 浏览器加载

浏览器加载ES6模块，使用`<script>`标签，但是要加入`type="module"`属性
外链js文件

```
<script type="module" src="index.js"></script>
```

也可以内嵌在网页中

```
<script type="module">
import utils from './utils.js';
// other code
</script>
```

对于加载外部模块，需要注意：

- 代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
- 模块脚本自动采用严格模式，不管有没有声明`use strict`。
- 模块之中，可以使用`import`命令加载其他模块（.js后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用`export`命令输出对外接口。
- 模块之中，顶层的`this`关键字返回`undefined`，而不是指向`window`。也就是说，在模块顶层使用`this`关键字，是无意义的。
- 同一个模块如果加载多次，将只执行一次。



## Node加载

Node要求 ES6 模块采用`.mjs`后缀文件名。也就是说，只要脚本文件里面使用`import`或者`export`命令，就必须采用`.mjs`后缀名。
这个功能还在试验阶段。安装`Node V8.5.0`或以上版本，要用`--experimental-modules`参数才能打开该功能。

```
$ node --experimental-modules my-app.mjs
```

`Node`的`import`命令只支持异步加载本地模块(`file:`协议)，不支持加载远程模块。



# 总结

这篇文章主要描述了JavaScript的模块化历程，以史为鉴，可以知兴替。每种技术的出现都是为了解决一些痛点的，不要排斥新技术，也不要迷茫学习新技术，除非它能解决你的痛点。



# 参考

* [JavaScript模块化CommonJS/AMD/CMD/UMD/ES6Module的区别](https://www.cnblogs.com/weiqinl/p/9940549.html)
* [各模块化使用的例子](https://github.com/weiqinl/demo/tree/master/js-module)
* [Require.js](https://requirejs.org/)
* [Sea.js](https://github.com/seajs/seajs)
* [UMD](https://github.com/umdjs/umd)
* [ES6 Module](http://es6.ruanyifeng.com/#docs/module-loader)
* [JavaScript模块化七日谈](http://huangxuan.me/js-module-7day/#/)









