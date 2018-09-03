---
title: 理解jQuery的实现过程
date: 2018-08-22 15:22:16
tags:
---
### 自己封装jQuery函数的一个例子
```
window.jQuery = ???
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi

 <-*************->


window.jQuery = function (x) {
            let nodes = {}                                  //选出div并转为伪数组
            let temp = document.querySelectorAll(x)
            for (let i = 0; i < temp.length; i++) {
                nodes[i] = temp[i]
                nodes.length = temp.length
            }                                

            nodes.addClass = function (y) {                //函数实现遍历div加上class的功能
                for (let i = 0; i < nodes.length; i++) {
                    nodes[i].classList.add(y)
                }
            }                              

            nodes.setText = function (text) {              //函数实现遍历div加上innerText的功能
                for (let i = 0; i < nodes.length; i++) {
                    nodes[i].innerText = text
                }
            }

            return nodes
        }
 window.$ = jQuery
 var $div = $('div')
 $div.addClass('red')
 $div.setText('hi')
 ```
 ### 代码实现过程
 ```
 window.jQuery = function(nodeOrSelector)
 ```
+ 一般我们的得到的元素都是一个dom对象，而这个对象最终继承的就是Node接口
+ 所以可以在 Node.prototype 上添加以上 addClass 及setText 方法
+ 但是Node.prototype 是一个共有对象，每个人写的程序都不一样， 如果大家都在这里面操作添加删除API , 就会出现各种问题
+ 所以我们可以取一个我们自己喜欢的名字来当作我们简便操作dom树，这个构造函数就不会被别人乱修改啦，而jQuery的作者就取名叫jQuery的
+ 为了更懒更简便的操作，所以让$ = jQuery
```
let nodes = {}
  if (typeof nodeOrSelector === 'string') {
    let tmp = document.querySelectorAll(nodeOrSelector);
    for(var i =0; i < tmp.length; i++){
      nodes[i] = tmp[i];
    }
    nodes.length = i;
  } else if (nodeOrSelector instanceof Node) {
    nodes = {
      0: nodeOrSelector,
      length: 1
    }
  }
  ```
+ 因为jQuery 传入的有可能是字符串形式的选择器 或者 节点元素，所以 这段代码实现的功能是
+ 先创建一个对象
- 判断传入的参数是字符串还是dom对象
- 如果是字符串就document.querySelectorAll匹配所有符合的选择器的元素，然后改成jQuery形式的对象，也就是我们开始创建的对象，不能直接复制给这个对象，因为匹配得到的对象是NodeList对象，是dom类型对象
- 如果是节点元素就直接添加进第一项就好了
- 因为jQuery ,$div[0] === div
```
nodes.addClass = function(classs) {
    for (let i = 0; i < nodes.length; i++) {
      nodes[i].classList.add(classs);
    }
  }
  nodes.setText = function(text){
    for (let i = 0; i < nodes.length; i++) {
        nodes[i].textContent = text;
      }
  }
  return nodes;
  ```
- 最后就是给这个要返回的对象添加API啦
- 真正的jQuery的API都是封装在jQuery.prototype中，所有的jQuery对象都共用这些API
- 这里的nodes就与两个方法之间产生了闭包， 保护了nodes的私有化
```
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('222') // 可将所有 div 的 textContent 变为 hi
```
- 为了更方便的操作 用```$```代替jQuery ，```var $div = $('div')``` 可以 ```var $div = jQuery('div')``` 也是一样的
- 构造函数返回的对象就包含了两个方法， 我们就可以直接调用啦。