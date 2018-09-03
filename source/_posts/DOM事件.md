---
title: DOM事件
date: 2018-08-25 14:38:54
tags:
---
# 1. DOM0时代html和js的那些事儿
<html\>
```
<head>
    <script>
        function print() {
            console.log('hi');
        }
    </script>
</head>
<body>
  <button id="x" onclick="print">a</button>  //html里的onclick相当于eval('print') ，这样只会打印出print这个函数
  <button id="y" onclick="print()">b</button>  //'hi'
  <button id="z" onclick="print.call()">c</button>  //'hi'
</body>
```
<js\>
```
x.onclick = print;  //js里这样是执行函数，输出'hi'
x.onclick = print();  //这样写是得到函数的返回值，undefined
x.onclick = print.call();  //同上，undefined
```
# 2. DOM2 
### 1. 监听事件
```
xxx.onclick = function(){};  //不要用这个，因为可能会覆盖掉别人的事件
//用下面这个

function f1(){}

xxx.addEventListener('click',f1)    //EventListener是队列，先绑定的就先触发
xxx.removeEventListener('click',f1);
```
若想让事件被点一次后就取消监听，可以将f1写成下面这样
```
function f1(){
  console.log(1);
  xxx.removeEventListener('click',f1);  //这样f1执行一次后就自动取消xxx的监听，
}

xxx.addEventListener('click',f1)    
```
这就是$btn.one('click',fn);

### 2. 嵌套的div被点击的顺序
![HTML](/images/HTML.png)![JavaScript](/images/JavaScript.png)
- **面试题1:**请问点击儿子的div，这三个函数的执行顺序是什么？
![内部执行顺序](/images/内部执行顺序.png)
默认的顺序是像右边的箭头序列，先最里面的儿子再爸爸再爷爷，但如果给addEventListener加个true的话，事件就会到左边来，如下
```
grand1.addEventListener('click',function fn1(){
    console.log('爷爷')
},true)
```
![addEventListener(true)](/images/true.png)
这样执行顺序就是爷爷 -> 儿子 -> 爸爸。
左边的（加true的顺序），叫做捕获阶段，右边叫冒泡阶段。
- **面试题2：若给同一个div加上true和false呢？**
```
child1.addEventListener('click',function fn1(){
    console.log('儿子冒泡')
},false)
child1.addEventListener('click',function fn1(){
    console.log('儿子捕获')
},true)
//点击child出现'儿子捕获','儿子冒泡'
//若是给被点击的本身加上true和false，则按事件添加的顺序执行
//若给爸爸或是爷爷添加true和false，则是按上图箭头顺序执行
```
- stopPropagation()
这个方法可以让它那一层停止往上传播
```
child1.addEventListener('click',function fn1(e){
    e.stopPropagation();
})  //这样点击儿子，就只会执行儿子，不会执行爸爸和爷爷
```
### 3. 点击别处关闭浮层
html
```
<div id="wraper" class="wraper">
    <button id="clickMe">点我</button>
    <div id="popover" class="popover">
        <input type="checkbox">浮层
    </div>
</div>
```

错误示范1，js
```
$(clickMe).on('click',function () {
    $(popover).show();
});
$(wrapper).on('click',false);  
//这里有错误，浮层出来后无法点击checkbox，因为这里的false，阻止了所有默认事件，应该改成下面那样
$(document).on('click',function () {
    $(popover).hide();
});
```
更新版1，还是不完美
```
$(clickMe).on('click',function () {
    $(popover).show();
});
$(wrapper).on('click',function(e){
    e.stopPropagation();    //阻止冒泡
});
$(document).on('click',function () {
    $(popover).hide();   
});    //这样若有多个浮层，则会监听多次document，浪费资源，可以改成下面的
```
错误示范2，js
```
$(clickMe).on('click',function () {
    $(popover).show();
    $(document).one('click',function () {  //改为one
      $(popover).hide();   
    });  
});    //若没有阻止冒泡，浮层不会显示出来，因为hide也会执行

//可以添加一个setTimeout使它不会马上执行，如下
$(clickMe).on('click',function () {
    $(popover).show();
    setTimeout(function(){
      $(document).one('click',function () {  
        $(popover).hide();   
      });  
    },0)    //添加setTimeout后，document.onclick不会马上执行，当下一次点击屏幕的时候才会执行
//这样还是有bug，点击popover内部也会执行hide()
});
```

完美版本1
```
$(clickMe).on('click',function () {
    $(popover).show();
    $(document).one('click',function () { 
        $(popover).hide();   
    });  //只在浮层show的时候监听document一次
});
$(wrapper).on('click',function(e){
    e.stopPropagation();  //阻止冒泡，这样点击popover内部就不会执行document.onclick，但这个事件还是会存放在那里
});
```
完美版本2
更新上个版本点按钮不会关闭浮层的bug
```
$(clickMe).on('click',function () {
    if($(popover).is(':hidden')){  //若浮层是关闭的，点击按钮则打开
        $(popover).show();
    }else {
        $(popover).hide();  //和上面相反
    }
    $(document).one('click',function () {
        $(popover).hide();
    });
});
$(wrapper).on('click',function(e){
    e.stopPropagation();  
});
```
