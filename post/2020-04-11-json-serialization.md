---
layout: post
title: JSON序列化问题
categories: JSON
description: JSON序列化问题
keywords: JSON
---

> 背景：接口返回的对象中有一个属性是JSON转译后的字符串需要转成JSON对象



开始直接将返回的数据序列化
```js
let data = {dictValue: "{\"code_x\":\"100\",\"code_y\":\"100\",\"bgimage_x\":\"30\",\"bgimage_y\":\"525\"}", dictName: "sharebackground.png", dictId: "1"}  // 接口返回的数据


JSON.parse(data.dictValue) // 报错

```
使用浏览器的调试窗口发现可以执行，分析出原因可能是取值时，需要序列化的字符串少了双引号，于是尝试如下操作

```js
let data = {dictValue: "{\"code_x\":\"100\",\"code_y\":\"100\",\"bgimage_x\":\"30\",\"bgimage_y\":\"525\"}", dictName: "sharebackground.png", dictId: "1"}  // 接口返回的数据


JSON.parse(`"${data.dictValue}"`) // 返回字符串的JSON对象

最终解决方案 JSON.parse(JSON.parse(`"${data.dictValue}"`))

```
