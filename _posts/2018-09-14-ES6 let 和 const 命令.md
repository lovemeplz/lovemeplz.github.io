---
layout:     post
title:      ES6
subtitle:   let 和 const 命令
date:       2018-9-14
author:     Shenzq
header-img: img/post-bg-ios9-web.jpg
catalog:   true
tags:
    - JavaScript
    - ES6
---

## let 命令

> es6新增了 `let` 命令,用来声明变量。它的用法类似于 `var` ,但是所有声明的变量,只有在 `let` 命令所在的代码块内有效

```
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

#### 不存在变量提升

> `var` 命令会发生”变量提升“现象，即变量可以在声明之前使用，值为`undefined `。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。为了纠正这种现象，`let` 命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;

```

#### 暂时性死区

> 只要块级作用域内存在 `let` 命令,它所声明的变量就'绑定'(binding)在这个区域,不再受外部影响

```
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}

```

#### 不允许重复声明

> let不允许在相同作用域内，重复声明同一个变量。

```
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}

```
> 因此,不能在函数内部重新声明参数

```
function func(arg) {
  let arg; // 报错
}

function func(arg) {
  {
    let arg; // 不报错
  }
}

```

#### 块级作用域

> ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。第一种场景，内层变量可能会覆盖外层变量。

```
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined

//上面代码的原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。
//但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

```

> 第二种场景，用来计数的循环变量泄露为全局变量。

```
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5

//上面代码中，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。
```

#### ES6的块级作用域

```
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}

```


## const 命令

> `const` 声明一个只读的常量。一旦声明，常量的值就不能改变。

```
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.

```
> const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。其余特性类似以`let`

#### 本质

> `const` 实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

```
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only

//上面代码中，常量foo储存的是一个地址，这个地址指向一个对象。
//不可变的只是这个地址，即不能把foo指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。
```
> 如果真的想将对象冻结，应该使用Object.freeze方法。

```
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;

//上面代码中，常量foo指向一个冻结的对象，所以添加新属性不起作用，严格模式时还会报错。
//除了将对象本身冻结，对象的属性也应该冻结。下面是一个将对象彻底冻结的函数。
```

```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

#### ES6 声明对象的6种方法

> ES5 有两种, `var` 和  `function`,ES6增加了 `let` `const` `import` `class`。





