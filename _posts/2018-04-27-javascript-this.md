---
layout: post
title: Javascript中this的指向
categories: Javascript
description: Javascript中this的指向
keywords: Javascript
---
### 普通函数

1.this总是指向它的直接调用者

2.在默认情况下(非严格模式)没有找到直接调用者，this指向当前容器

3.严格模式下，没有直接调用函数的this为undefined

4.call,apply,bind绑定的this指的是绑定对象


#### 1、指向直接调用者

```javascript
function test(){
    console.log(this);
};
test();

结果：window
```

解释：这里的test指向 window.test();这里的调用者是window,所以这里的this指向window

<h4>2、严格模式下</h4>

```javascript
function test(){
    'use strict';
    console.log(this);
}
test();

结果：undefined
A268809967
```

<h3>匿名函数、定时器中的函数</h3>

1.匿名函数和定时器中的函数直接指向window

```javascript
(function(){
    console.log(this);
})()

结果：window

function test(){
    setTimeout(function(){
        console.log(this);
    },1000)
}
test();

结果：window对象

```

<h3>箭头函数</h3>

箭头函数内部的this是词法作用域，由上下文确定

```javascript
let abc = [];
abc.push({
    name:'abc'
});
abc.forEach((key,value)=&gt;{
    console.log(this);
})

结果：window对象
```
