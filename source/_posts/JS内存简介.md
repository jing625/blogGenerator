---
title: Js内存简介
date: 2018-08-12 22:56:14
tags:
---
![内存](/images/内存.png)

## 下面介绍浏览器里js的内存
1. 除了对象类型，其他类型的数据是直接存在stack里

2. 存对象的时候，将数据存在Heap（堆内存）里，并将数据在Heap中的位置信息存在stack（栈内存）里，由于Heap里存数据不是按顺序存的，所以很方便对象随时添加属性。当把对象赋给另一个对象时，是把这个对象的地址赋给它。
所以用对象时是引用的对象的地址，这种关系叫引用

3. 8g的内存相当于 8 * 1024 * 1024 * 1024 * 8 =  6.87E10  bit
而64位二进制的数相当于 2^64^ = 1.84E19，所以64位二进制能用来存内存里的所有位置

4. 数字是64位的，字符是16位的（ES3）

## 5. 面试题

**1.引用自身**

错误实例：
   ```
    var a = {self : a};     //这样a.self会是undefined
    
    上面这样等于
    var a;                  //undefined
    a.self = a              //这时a还是是undefined
    因为赋值时要先确定右边的
  ```
   正确写法：
  ```
   var a = {};
   a.self = a;            //这样a.self就等于它自己
  ```
**2**
![面试2.png](/images/面试2.png)

```
var a = {n:1};
var b = a;
a.x = a = {n:2}   //这一行运行的时候最左的a地址已经确定为最开始的地址，于是先运行a = {n:2}，相当于给了a一个新地址，再运行a.x = a，相当于把新地址赋给了旧地址上对象一个x属性

alert(a.x);       // undefined，然而有了新地址的a并没有x属性
alert(b.x);       // [object,Object]，b是引用的a的旧地址，所以b有
```

**3. 垃圾、垃圾回收**
```
var a = function(){};
var a = null;

 这样上面那个function因为没有被任何东西引用，所以function占的内存就是垃圾，会被浏览器回收

```
下面是面试题
```
var fn = function(){};
document.body.onclick = fn;
fn = null;
```
请问function是垃圾吗？
答案不是，因为`document.body.onclick = fn;`还在引用
![面试3](/images/面试3.png)

想要function变成垃圾可写
`document.body.onclick = null;`
附加：若是把页面关了，就相当于document不存在了，于是这时body、onclick、function都是垃圾，浏览器会把它们回收，但IE6不会把它们当作垃圾，IE6有bug！这就叫内存泄露，垃圾会占满内存，造成卡顿，解决办法：
```
window.onunload = function(){
    document.body.onclick=  null;
}
```