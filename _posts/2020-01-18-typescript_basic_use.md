---
layout: post
title: TypeScript基础语法
categories: TypeScript
description: TypeScript基础语法
keywords: TypeScript
---

### 数据类型
```javascript
// 字符串类型
let str:string = 'test';

// 数字类型
let num:number = 123;

// 布尔类型
let flag:boolean = true;

// 多重数据类型
let strOrUndefined:string|undefined;

// null类型只能为null
let strNull:null;
// strNull = {}; 报错
strNull = null;

// 数组类型
let arr:number[] = [1,2,3];
let arr1:Array<Number> = [1,2,3];
let arr2:[number:string] = [1,'aa'];
```
