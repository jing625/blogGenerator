---
layout: javascript
title: Javascript 中 apply、call、bind的理解
date: 2018-08-28 13:16:29
tags:
---
###1.apply、call
在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。
JavaScript 的一大特点是，函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。
```
function fruits() {}
 
fruits.prototype = {
    color: "red",
    say: function() {
        console.log("My color is " + this.color);
    }
}
 
var apple = new fruits;
apple.say();    //My color is red
```
但是如果我们有一个对象banana= {color : “yellow”} ,我们不想对它重新定义 say 方法，那么我们可以通过 call 或 apply 用 apple 的 say 方法：
```
banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow

banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow
```
所以，可以看出 call 和 apply 是为了动态改变 this 而出现的，当一个 object 没有某个方法（本栗子中banana没有say方法），但是其他的有（本栗子中apple有say方法），我们可以借助call或apply用其它对象的方法来操作。
###2.apply、call 的区别
对于 apply、call 二者而言，作用完全一样，只是接受参数的方式不太一样。例如，有一个函数定义如下：
```
var func = function(arg1, arg2) {
 
};
```
就可以通过如下方式来调用：
```
func.call(this, arg1, arg2);
func.apply(this, [arg1, arg2])
```
其中 this 是你想指定的上下文，他可以是任何一个 JavaScript 对象(JavaScript 中一切皆对象)，call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。

JavaScript 中，某个函数的参数数量是不固定的，因此要说适用条件的话，当你的参数是明确知道数量时用 call 。
而不确定的时候用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个数组来遍历所有的参数。
为了巩固加深记忆，下面列举一些常用用法：
1. 数组之间追加
```
var array1 = [12 , "foo" , {name "Joe"} , -2458]; 
var array2 = ["Doe" , 555 , 100]; 
Array.prototype.push.apply(array1, array2); 
/* array1 值为  [12 , "foo" , {name "Joe"} , -2458 , "Doe" , 555 , 100] */
```
2. 获取数组中的最大值和最小值
```
var  numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
    maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
```
number 本身没有 max 方法，但是 Math 有，我们就可以借助 call 或者 apply 使用其方法。
3. 验证是否是数组（前提是toString()方法没有被重写过）
```
functionisArray(obj){ 
    returnObject.prototype.toString.call(obj) === '[object Array]' ;
}
```
4. 类（伪）数组使用数组方法
```
var domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));
```
Javascript中存在一种名为伪数组的对象结构。比较特别的是 arguments 对象，还有像调用 getElementsByTagName , document.childNodes 之类的，它们返回NodeList对象都属于伪数组。不能应用 Array下的 push , pop 等方法。
但是我们能通过 Array.prototype.slice.call 转换为真正的数组的带有 length 属性的对象，这样 domNodes 就可以应用 Array 下的所有方法了。
###3.深入理解运用apply、call
定义一个 log 方法，让它可以代理 console.log 方法，常见的解决方法是：
```
function log(msg)　{
  console.log(msg);
}
log(1);    //1
log(1,2);    //1
```
上面方法可以解决最基本的需求，但是当传入参数的个数是不确定的时候，上面的方法就失效了，这个时候就可以考虑使用 apply 或者 call，注意这里传入多少个参数是不确定的，所以使用apply是最好的，方法如下：
```
function log(){
  console.log.apply(console, arguments);
};
log(1);    //1
log(1,2);    
```
接下来的要求是给每一个 log 消息添加一个”(app)”的前辍，比如：
```
log("hello world");    //(app)hello world
```
该怎么做比较优雅呢?这个时候需要想到arguments参数是个伪数组，通过 Array.prototype.slice.call 转化为标准数组，再使用数组方法unshift，像这样：
```
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');
 
  console.log.apply(console, args);
};
```
###4.bind
说完了 apply 和 call ，再来说说bind。bind() 方法与 apply 和 call 很相似，也是可以改变函数体内 this 的指向。

MDN的解释是：bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

直接来看看具体如何使用，在常见的单体模式中，通常我们会使用 _this , that , self 等保存 this ，这样我们可以在改变了上下文之后继续引用到它。 像这样：
```
var foo = {
    bar : 1,
    eventBind: function(){
        var _this = this;
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(_this.bar);     //1
        });
    }
}

作者：紫陌兰溪
链接：https://www.jianshu.com/p/307740bc07cb
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
```
由于 Javascript 特有的机制，上下文环境在 eventBind:function(){ } 过渡到 $(‘.someClass’).on(‘click’,function(event) { }) 发生了改变，上述使用变量保存 this 这些方式都是有用的，也没有什么问题。当然使用 bind() 可以更加优雅的解决这个问题：
```
var foo = {
    bar : 1,
    eventBind: function(){
        $('.someClass').on('click',function(event) {
            /* Act on the event */
            console.log(this.bar);      //1
        }.bind(this));
    }
}
```
在上述代码里，bind() 创建了一个函数，当这个click事件绑定在被调用的时候，它的 this 关键词会被设置成被传入的值（这里指调用bind()时传入的参数）。因此，这里我们传入想要的上下文 this(其实就是 foo )，到 bind() 函数中。然后，当回调函数被执行的时候， this 便指向 foo 对象。再来一个简单的栗子：
```
var bar = function(){
    console.log(this.x);
}
bar(); // undefined
var func = bar.bind(foo);
func(); // 3
```
这里我们创建了一个新的函数 func，当使用 bind() 创建一个绑定函数之后，它被执行的时候，它的 this 会被设置成 foo ， 而不是像我们调用 bar() 时的全局作用域。

有个有趣的问题，如果连续 bind() 两次，亦或者是连续 bind() 三次那么输出的值是什么呢？像这样：
```
var bar = function(){
    console.log(this.x);
}
var foo = {
    x:3
}
var sed = {
    x:4
}
var func = bar.bind(foo).bind(sed);
func(); //?
 
var fiv = {
    x:5
}
var func = bar.bind(foo).bind(sed).bind(fiv);
func(); //?
```
答案是，两次都仍将输出 3 ，而非期待中的 4 和 5 。原因是，在Javascript中，多次 bind() 是无效的。更深层次的原因， bind() 的实现，相当于使用函数在内部包了一个 call / apply ，第二次 bind() 相当于再包住第一次 bind() ,故第二次以后的 bind 是无法生效的。
###5.apply、call、bind比较
1. bind、call和apply方法的相同之处
前面我们已经说了call和apply的作用相同，区别就在参数的形式不同。bind方法的作用也是改变this的指向。bind方法的参数要求和call方法一样，第一个参数是函数执行上下文的对象，后面的参数可以是多个。
　　但bind又有自己的特别之处，下面来说说bind的用法上的区别：
2. bind方法的用法上的区别
- 区别一： bind()方法是会创建一个新函数，当调用这个新函数时，会以创建新函数时传入 bind()方法的第一个参数作为 this。
看栗子来体会：
```
var name='xxx'; 
var obj = {
    name: 'yyy'
}
function func() {
    console.log(this.name);
}
var func1 = func.bind(obj);
func1();   //yyy
func();     //xxx
```
func1等于通过bind方法创建的和func函数相同的新函数。func1的this对象为传入的obj，所以this.name就只等于obj对象的name值‘yyy’。
　　因为是新生成的函数，所以原来函数func()并不受影响，所以直接执行func()，this指向全局window，所以this.name为'xxx'。
- 区别二： 参数的使用上，call方法是将第二个及之后的参数，作为实参传递到函数中。
　　而 bind() 方法的第二个以及以后的参数，再加上新函数运行时的参数，按照顺序作为实参传递到新函数中。
　　这样看文字解释，真的太绕了，直接看栗子，就很好理解：
```
function func(a, b, c) {
    console.log(a, b, c);
}
var func1 = func.bind(null,'aaa');

func('A', 'B', 'C');            // A B C
func1('A', 'B', 'C');           // aaa A B
func1('B', 'C');                // aaa B C
func.call(null, 'aaa');      // aaa undefined undefined
```
- 区别三：写法上，bind函数需要再加个()
最后一个不同，就是bind函数不是立即调用的，实现生成新的处理函数，然后再执行。而call和apply方法都是立即执行。
　　还是看栗子吧，改写下前面的例子：（一样的实现效果，不一样的执行写法）
```
var name='xxx'; 
var obj = {
    name: 'yyy'
}
function func() {
    console.log(this.name);
}
console.log(func.bind(obj());  //yyy
console.log(func.call(obj));    //yyy
console.log(func.apply(obj));  //yyy
```
3. bind方法常见的应用写法
- 例子１：改变setTimeout()方法的this指向
```
<body>
    <button class="bindtest">dianwo</button>
    <script>
    /*bind的用法--和使用var _this=this达到同样的目的*/
        bindtest=document.querySelector('.bindtest')
        bindtest.addEventListener('click',function(){
            console.log(this) //<button class="domtest">dianwo</button>
            setTimeout(function(){
                console.log(this)   //<button class="domtest">dianwo</button>
            }.bind(this),300)
        })
    </script>
</body>
```
前面说了，bind的作用是得到一个新的函数，而新函数的this就是bind传递的第一个参数。这里bind方法是和setTimeout方法同级的，属于bindtest对象下的点击事件下的，所以this是 bindtest对象。
- 例子２：字符串拼接的bind方法实现
　　前面call方法的使用例子中实现的字符串拼接，现在换成用bind方法来实现：
　　常见的是，比如我需要一个join操作，但是我没有这个方法，那么我就需要借助数组Array原型的join方法来借位使用。
```
function joinStr(){
      var joins=Array.prototype.join.bind(arguments);
      console.log(joins('-'))
}
joinStr('a','b','c')
```
例子中的写法等同于['a','b','c'].join('-')


