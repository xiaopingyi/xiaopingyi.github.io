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

// 枚举类型
enum testEnum {success:1,error:0};
let s:testEnum = testEnum.success;
let e:testEnum = testEnum.error;

// 任意数据类型
let strAny:any;
strAny = 1;
strAny = 'abc';

// void类型
// 这里的void指无返回值
funciton test():void{
}
```
总结：typeScript中定义变量需要 变量作用域+变量名称:变量类型，而javascript中无需定义变量的类型

### 函数
```javascript
// 函数的定义

// 函数式声明
ts写法：function test1(x:string,y:string){}
编译后：function test1(x,y){}

// 函数表达式
ts写法：let test2 = (x:string,y:string){}
编译后：var test2 = function(x,y){}

// 函数表达式：指定变量fn的类型
ts写法：let test3:(x:string,y:string) => (string) = (x,y) => {
  return x+y;
}
编译后：var test = function(x,y){
  return x+y;
}

// 返回为空
ts写法:let test5 = function(x:string):void{}
编译后：let test5 = function(x){}

// 可选参数
ts写法:let test6 = function(x?:string,y?:string){}
编译后:let test6 = function(x,y){}

// 默认值
ts写法：let test7 = function(x:number = 10,y:string:='abc'){}
编译后：
let test7 = function(x,y){
  if(x == void 0) { x == 10;}
  if(y == void 0) { y == 'abc'}
}

// 扩展运算符
ts写法：let test8 = fucntion(...arr:number){}
编译后：
let test8 = function(){
  let arr = [];
  for(var _i=0;_i< < arguments.length; _i++){
    arr[_i] = 
  }
}
```
