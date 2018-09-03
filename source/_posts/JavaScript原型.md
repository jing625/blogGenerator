---
title: JavaScript原型
date: 2018-08-09 12:47:25
tags:
---
# 原型
学习Javascript之初总是被__proto__ 与 prototype 这两个属性绕晕了， 所以先在此列出常见的各指向情况：
```
var num = 1
   num.__proto__ === Number.prototype
   num.__proto__.__proto__ === Object.prototype
   num.__proto__.constructor === Number
   Number.prototype.__proto__ === Object.prototype
   Number.prototype.constructor === Number
   
   var fn = function(){}
   fn.__proto__ === Function.prototype
   fn.__proto__.__proto__ === Object.prototype
   fn.__proto__.constructor === Function
   Function.prototype.__proto__ === Object.prototype
   Function.prototype.constructor === Function

   var array = []
   array.__proto__ === Array.prototype
   array.__proto__.__proto__ === Object.prototype
   array.__proto__.constructor === Array
   Array.prototype.__proto__ === Object.prototype
   Array.prototype.constructor === Array

   var bool = true
   bool.__proto__ === Boolean.prototype
   bool.__proto__.__proto__ === Object.prototype
   bool.__proto__.constructor === Boolean/**/
   Boolean.prototype.__proto__ === Object.prototype
   Boolean.prototype.constructor === Boolean

   var str = "String"
   str.__proto__ === String.prototype
   str.__proto__.__proto__ === Object.prototype
   str.__proto__.constructor === String
   String.prototype.__proto__ === Object.prototype
   String.prototype.constructor === String
   
   var object = {}
   object.__proto__ === Object.prototype
   object.__proto__.__proto__ === null
   object.__proto__.constructor === Object
   Object.prototype.__proto__ === null
   Object.prototype.constructor === Object
```
运行都为true，从以上代码一步步讲解实例，原型，原型链，构造函数，构造函数属性，new指令 及 js一切皆对象的误解

## 1.构造函数
JS中以上代码会用到的几个构造函数分别是
```
Function，Number，Boolean，String，Object
```
而这些构造函数都是由 Function构造出来的， 所以
```
Function.__proto__ === Function.prototype // 为 true
Array.__proto__ === Function.prototype // 为 true
Object.__proto__ === Function.prototype // 为 true
Number.__proto__ === Function.prototype
Boolean.__proto__ === Function.prototype
String.__proto__ === Function.prototype
```
以上第一句代码 var num = 1 其实应该这样写 var num = new Number(1) , 那么为什么 直接赋值num还是可以使用很多Number的方法呢

因为JS在使用num的时候会创建一个临时的Number（1）给num ，然后num就可以正常使用Number的方法，本质上num还是基础数据类型，而不是混合数据类型的对象形式
那么为什么要使用new呢
当一个函数New的时候 会执行以下几步
```
//使用new 会有以下四个步骤
   function fn(){
        1.  创建一个临时对象
        var temp  = {}

        2.  临时对象__proto__ =  fn.原型
        temp.__proto__ = fn.prototype

        3. 返回这个临时对象
        return temp 
   }

   fn.prototype = {
       constructor ： fn
   }
```
那么执行完 var num = new Number(1) 后 num就是一个对象了， 而这个对象里面有一个__proto__的属性 。

## 2. 实例
所以实例就是： 执行构造函数后得到的这个对象

## 3. 原型
而原型就是： 实例__proto__属性指向的 该函数的 prototype属性也是一个对象

## 4. 构造函数属性
构造函数属性constructor指向该构造函数本身

## 5. 原型链
而 原型链就是 num.__proto__.__proto__.__proto__ === null 这条链所对应的值

## 6. 读取属性
当我们读取属性时， 从实例对象的属性开始找，如果实例属性中没有需要的属性，就去原型中找，如果该原型中还是没有，又会继续往下一层原型中找，直到顶层为止。 num.__proto__.__proto__.__proto__ === null只想null的时候

## 7. 总结
所有的构造函数都是由Function构造函数所构造出来的 ```Array.__proto__ === Function.prototype```
所有的实例对象的```__proto__```属性都指向该构造函数的```prototype属性array.__proto__ === Array.prototype```
所有的实例对象的```__proto__.__proto__```属性都指向的构造函数```Object```的```prototype```属性```num.__proto__.__proto__ === Object.prototype```
所有的原型的```constructor```属性都指向该构造函数```num.__proto__.constructor === Number```
所有```Number.prototype.__proto__ === Object.prototype```
Object的实例对象的```__proto__.__proto__ ```为null