---
title: DOM面试
date: 2018-08-25 23:50:25
tags:
---
# 1. [DOM事件模型](https://www.jianshu.com/p/7054b1abb431)
1. 冒泡
2. 捕获
3. [如果这个元素是被点击元素，那么执行顺序由监听顺序决定](http://jsbin.com/raqakog/1/edit?js,console,output)

# 2. 移动端的触摸事件
1. 
```
//说明触摸开始
document.body.ontouchstart !== undefined
//触摸开始移动
canvas.ontouchmove = function (ev) {
  var x = ev.touches[0].clientX;  //触摸的x坐标
  var y = ev.touches[0].clientY;
}
ontouchend  //触摸结束
ontouchcancel  //触摸取消，因为有的浏览器不会触发touchend事件
```
2. 模拟swipe事件（滑动）
记录两次touchmove的位置差，若后一次在前一次的右边，说明向右滑了

# 3. 事件委托
1. 监听父元素，看被触发的是哪个儿子，就是事件委托，委托父亲监听儿子
2. 优点是可以监听动态生成的儿子，减少了事件处理程序的数量，整个页面占用的内存空间会更少。
```
function listen(element,eventType,selector,fn){
  element.addEventListener(eventType,(e)=>{
    let el = e.target
    while(!el.matches(selector)){  //可能点击的是li里的span  
      if(element === el){  //如果点击的是父元素，则跳出监听
        el =null
        break
      }
      el = el.parentNode
    }
    el&&fn.call(el,e,el)
  })
  return element
}

//<ul id="list">
//  <li></li>
//  ...
//</ul>
//let list=document.querySelector('#list')
//listen(list,'click','li',()=>{})
```

