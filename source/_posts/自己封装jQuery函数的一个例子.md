---
title: 实现一个jQuery的API
date: 2018-08-28 15:22:16
tags:
---
自己封装jQuery函数的一个例子
```
window.ffdom = {} /*yui*/
ffdom.getSiblings = function (node) { /* API */
  var allChildren = node.parentNode.children

  var array = {
    length: 0
  }
  for (let i = 0; i < allChildren.length; i++) {
    if (allChildren[i] !== node) {
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array
}
ffdom.addClass = function (node, classes) {
  classes.forEach( (value) => node.classList.add(value) )
}

ffdom.getSiblings(item3)
ffdom.addClass(item3, ['a','b','c'])

Node.prototype.getSiblings = function(){
  var allChildren = this.parentNode.children

  var array = {
    length: 0
  }
  for (let i = 0; i < allChildren.length; i++) {
    if (allChildren[i] !== this) {
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array
}
item3.getSiblings()


 <-*************->



window.jQuery = ???
window.$ = jQuery

var $div = $('div')
$div.addClass('red') // 可将所有 div 的 class 添加一个 red
$div.setText('hi') // 可将所有 div 的 textContent 变为 hi



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