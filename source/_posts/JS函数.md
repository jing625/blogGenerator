---
title: JS函数
date: 2018-08-20 13:27:06
tags:
---
# 1. 函数的声明方式
   ```
  1.function a(a,b){
       return a+b;
   }

   2. var a = function(a,b){    //匿名函数，函数表达式
       return a+b;
   }
   a(1,2)    //3

   3.var b = new Function(
       'a', //构造函数，    基本没人用
       'b',
       'return a+b'
   )；
   b(1,2)    //3

   var b = Function('a','b','return a+b')
   b(1,2)    //3，加new和不加new没区别

   var b = Function('return "hello world"');  
   //若只有一个参数，则就是函数体

   4.var e = (a,b) => {
       var m = a*2; 
       var n = b*2; 
       return m+n;
   }

   //若只有一句话，并且不返回对象，可以同时去掉return和{}
    var e = (a,b) => a+b

   //若只有一个参数，可以省略()
    var e = n => n*n
   ```
# 2. 若重复声明函数，**由于函数名的提升**，前一次声明在任何时候都是无效的，这一点要特别注意。如下
    
    function f() {
      console.log(1);
    }
    f() // 2

    function f() {
      console.log(2);
    }
    f() // 2

   函数的变量提升：
   ```
f();    //不会报错，因为下面的变量提升了

    function f() {}    
   ```
   ```
   f();
   var f = function (){};
   // 报错，TypeError: undefined is not a function
   // 因为上面这两行等同于下面
   var f;
   f();
   f = function () {};
   ```
   所以如果同时采用function命令和赋值语句声明同一个函数，最后总是采用赋值语句的定义。
   ```    
var f = function () {
     console.log('1');
   }

   function f() {
     console.log('2');
   }

   f() // 1，相当于下面
    
   var f;
   function f() {
     console.log('2');
   }
   f = function () {
      console.log('1');
   }
   f();    //1
   ```
# 3. 注意：不要在if语句和try语句里使用函数声明
   ```
   if (false) {
     function f() {}    //即使是false，f函数也会被声    明，因为函数名的提升
   }

   f() // 不报错，因为函数名提升，所以不要在if    语句里使用函数声明

   //但可以使用函数表达式
   if (false) {
     var f = function () {};
   }

   f() // undefined
   ```
# 4. 函数的属性
name
   ```
   function f1() {}
   f1.name    // "f1"，name属性

   var f2 = function(){};
   f2.name    //f2

   var f3 = function f4(){};
   f3.name    //f4

   var f5 = new Function('x','y','return x+y');
   f5.name    //'anonymous'，匿名的
   ```
   length  
   ```
   function f(a, b) {}
   f.length // 2，函数的参数个数
   ```
   toString
   ```
   function f(){
       console.log('可以的');
   }
   f.toString();    // "function f(){
                    //    console.log('可以的');                     //  }"
   //利用这一点可以变相实现多行字符串
   function f() {/*
     这是一个
     多行注释
   */}

   f.toString()
   // "function f(){/*
   //   这是一个
   //   多行注释    
// */}"
  ```
   [变相实现多行字符串](http://javascript.ruanyifeng.com/grammar/function.html#toc10)

# 5. 局部变量
在函数内部声明的变量是局部变量，在函数以外声明的全是全局变量，在其他区块中声明的一律也是全局变量。

# 6. 函数内部也有变量提升！

# 7. 函数的作用域，就是其声明时所在的作用域，与其运行时所在的作用域无关。
   ```
   var a = 1;
   var x = function () {
     console.log(a);
   };

   function f() {
     var a = 2;
     x();
   }

   f() // 1
   ```
# 8. 传递方式
   ```
   var obj = { p: 1 };

   function f(o) {
     o.p = 2;
   }
   f(obj);  //相当于把obj的地址传进去

   obj.p // 2
   ```
   如果函数内部修改的，不是参数对象的某个属性，而是替换掉整个参数，这时不会影响到原始值。
   ```
   var obj = [1, 2, 3];

   function f(o) {
     o = [2, 3, 4];
   }
   f(obj);

   obj // [1, 2, 3]
   ```
   **这是因为，o的值实际是参数obj的地址，重新对o赋值导致o指向另一个地址，保存在原地址上的值不受影响。复合类型是传地址**
```
var p = 2;

function f(p) {
  p = 3;
}
f(p);  //基本类型，所以传进去的不是地址而只是p的值

p // 2
```
原始类型的值，传入函数只是值的拷贝，所以怎么修改都不会影响到原始值

# 9. arguments对象
可以通过argument对象获得参数
   ```
   function f(a, a) {
     console.log(arguments[0]);
   }

   f(1) // 1
   ```
   正常模式下，arguments对象还可以在运行时修改参数。严格模式下不行
   ```
   var f = function(a, b) {
     arguments[0] = 3;
     arguments[1] = 2;
     return a + b;
   }

   f(1, 1) // 5

   arguments.length //可以知道传入多少个参数
   ```
   arguments不是数组，但可以[转换为数组](http://javascript.ruanyifeng.com/grammar/function.html#toc20)

# 10. 返回值
函数即使没有return，JS会自动加上return undefined；

# 11. function的不一致性
   ```
   function y(){};
   y;    //f y(){}
    
   var x = function i(){};
   i;    //Error: i is not defined，i只能作用在它自己的函数里
   ```

# 12. 函数的调用
```
f(1,2);
f.call(undefined,1,2);  //这两种调用方式等价
```

# 13. this和arguments
**this是call()的第一个参数**
```
f = function(){
    console.log(this);
}
f.call(undefined);    //正常模式下，若传入的是null或undefined，打印出来的就是window

f = function(){
    'use strict'
    console.log(this);
}
f.call(undefined);    //严格模式打印出undefined
```
面试题1：
```
var obj = {
  foo: function(){
    console.log(this)
  }
}

var bar = obj.foo
obj.foo() // 打印出obj，因为转换为 obj.foo.call(obj)，this 就是 obj
bar()   //打印出window，转换为 bar.call()，由于没有传东西，所以 this 就是 undefined，最后浏览器给你一个默认的 this —— window 对象
```
面试题2:
```
function fn (){ console.log(this) }
var arr = [fn, fn2]
arr[0]() // 这里面的 this 又是什么呢？
```
我们可以把 arr[0]( ) 想象为arr.0( )，虽然后者的语法错了，但是形式与转换代码一样，于是：
```
arr.0.call(arr);
//this就是arr
```
**arguments是伪数组**
```
f = function(){
    console.log(arguments);
}
f.call(undefined,1,2,3);    //[1,2,3,......]
```
arguments有数组的一些特性，但没有在Array的原型链中，所以arguments没有数组的方法，如push
```
arguments.__proto__ === Array.prototype //false
```

# 14. 函数的作用域
**看到面试题先把代码改了，做变量提升！再做题**
## 面试题1：
```
var a = 1;
function f1() {
    console.log(a)
    var a = 2;
    f4.call();
}

function f4() {
    console.log(a);
}

f1.call();    //??
```
答案：undefined（f1的console.log）、1(f4的console.log)

## 面试题2:
```
var a = 1;
function f1() {
    console.log(a)
    var a = 2;
    f4.call();
}

function f4() {
    console.log(a);
}

a=2;
f1.call();    //？
```
答案：undefined、2

## 面试题3:
html
```
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
</ul>
```
JS
```
var liTags = document.querySelectorAll('li');
for(var i=0;i<liTags.length;i++){
    liTags[i].onclick = function () {
        console.log(i);
    }
}

//请问点3时，输出什么
```
答案：6
先JS变量提升
```
var liTags;
var i;
liTags = document.querySelectorAll('li');
for(i=0;i<liTags.length;i++){
    liTags[i].onclick = function () {
        console.log(i);
    }
}
```
因为点3时，for循环已执行完了，i变成6，所以输出的是6
## 面试题4
```
var x = function () {
  console.log(a);
};

function y(f) {
  var a = 2;
  f();
}

y(x)    // ?
```
答案:`a is not defined`
因为函数x在外部，不会引用y的内部变量
# 15. 闭包
```
var a = 1;

function f4() {
    console.log(a);
}
```
若一个函数使用了它作用域外的变量，那 (这个函数+变量) 就是闭包。
函数外部无法访问函数内部的变量，但使用闭包就可以
```
function f1() {
  var n = 999;
  function f2() {
    console.log(n);
  }
  return f2;
}

var result = f1();
result(); // 999，闭包就是f2
```
本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。
闭包的第二个作用就是让它读取的这些变量始终保持在内存中
```
function createIncrementor(start) {
  return function () {
    return start++;
  };
}

var inc = createIncrementor(5);

inc() // 5
inc() // 6
inc() // 7
```
闭包的另一个用处，是封装对象的私有属性和私有方法。
```
function Person(name) {
  var _age;
  function setAge(n) {
    _age = n;
  }
  function getAge() {
    return _age;
  }

  return {
    name: name,
    getAge: getAge,
    setAge: setAge
  };
}

var p1 = Person('张三');
p1.setAge(25);
p1.getAge() // 25
```
函数Person的内部变量_age，通过闭包getAge和setAge，变成了返回对象p1的私有变量。

# 16. 同名参数
```
function f(a, a) {
  console.log(a);
}

f(1, 2) // 2，以后面那个为准

function f(a, a) {
  console.log(a);
}

f(1) // undefined，以后面那个为准，若要取第一个，可以用arguments对象
```

# 17 .立即调用的函数表达式（IIFE）
```
function(){ /* code */ }();
// SyntaxError: Unexpected token (
```
因为JS把function在行首的，一律解释成语句，而语句后不应该出现()，最简单的处理，就是将其放在一个圆括号里面。
```
(function(){ /* code */ }());
// 或者
(function(){ /* code */ })();
//或者
-function(){ /* code */ }();
//或者
+function(){ /* code */ }();
//或者
!function(){ /* code */ }();
//或者
~function(){ /* code */ }();
//注意，上面前两种写法最后的分号都是必须的。
```
推而广之，任何以表达式来处理函数定义的方法，都是可以的

# 18. eval
eval命令的作用是，将字符串当作语句执行。
```
eval('var a = 1;');
a // 1
```
JavaScript 规定，如果使用严格模式，eval内部声明的变量，不会影响到外部作用域。
```
(function f() {
  'use strict';
  eval('var foo = 123');
  console.log(foo);  // ReferenceError: foo is not defined
})()
```
即使在严格模式下，eval依然可以读写当前作用域的变量。
```
(function f() {
  'use strict';
  var foo = 1;
  eval('foo = 2');
  console.log(foo);  // 2
})()
```