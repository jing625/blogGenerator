---
title: JavaScript Dom入门
date: 2018-08-16 23:32:07
tags:
---
DOM 目前的通用版本是DOM3
# 1. JS初级主要就两个作用：
>1.找元素
2.给元素增加/删除class

# 2. 节点API
- **1. nodeType**
    ```
    Node.ELEMENT_NODE === 1  //true，元素节点
    Node.TEXT_NODE === 3     //true，文本节点
    Node.COMMENT_NODE === 8 //true，注释节点    
    Node.DOCUMENT_TYPE_NODE === 10 //true，例：<!DOCTYPE html>
    Node.DOCUMENT_FRAGMENT_NODE === 11 
    //重点！

    xxx.nodeType === 3       //可用来检验xxx是一个元素节点还是元素中的文本
    ```

- **2. tagName**
ul的tagName为'UL'（大写！）

    ```
    let brother = li.getElementsByTagName('UL')[0];
    ```
    getElementsByTagName这个API搜索的是li元素的后代，并按顺序返回所有的ul，返回的是一个数组，要写上[0]

- **3. getAttribute**
    ```
    a.href     //这样获取href，浏览器会给href加上http协议
    a.getAttribute('href')  //这样获取的才是href本身
    ```

- **4. target和currentTarget**
    ```
    liTags[i].onmouseenter = function (x) {
          console.log(x.target);       //x.target是我们操作的那个元素，若a里包含了个span，那操作的就是span
          console.log(x.currentTarget) //x.currentTarget是我们监听的元素，就是liTags[i]这个元素
    }
    ```
- **5. 选择器**
    ```
    let a = document.querySelector('a[href="' + id + '"]')
    //href外要包一个中括号，地址外要包一个双引号！并且变量id要和其他的用加号隔开
    ```

- **6. 儿子children**
    ```
    var c = div.children;    //会获得div下的所有子标签
    var c = div.childNodes;  //会获得div下的所有子标签加文本标签
    ```

- **7. Siblings**
获取所有兄弟方法一：
```
var getSiblings = function (elem) {
var siblings = [];
var sibling = elem.parentNode.firstChild;
for (; sibling; sibling = sibling.nextSibling) {
	if (sibling.nodeType !== 1 || sibling === elem) continue;
	    siblings.push(sibling);
	}
	return siblings;
};
```
获取所有兄弟方法二 用getSiblings()：
```
var elem = document.querySelector('#some-element');
var siblings = getSiblings(elem);
```
注意：nextSibling如果有换行，会把换行当作下一个兄弟

- **8. nodeName()**
```
document.body.nodeName    //'BODY'，body的节点名字，(除了svg，其他节点名字返回都是大写）
document.documentElement.nodeName    //'HTML'
```
- **9. cloneNode()**
```
var div2 = div1.cloneNode();  //会把div1本身复制给div2
var div2 = div1.cloneNode(true);  //会把div1及它的所有子节点一起复制给div2，这句等于var div2=div1;
```
- **10. isSameNode()和isEqualNode()**
isEqualNode检测内容是否相同
isSameNode检测两个元素是否是同一个节点
- **11. removeChild()**
在页面里移除child节点，但是child在内存还是存在的
- **12. replaceChild()**
替换儿子，被替换的只是在页面里不见了，还是存在内存里的
- **13. normalize()**
将代码正常化 
- **14. innerText()和textContent()**

    innerText是IE的私有实现,但也被除FF之外的浏览器所实现,textContent 则是w3c的标准API,现在IE9也实现了。

     **区别1：**textContent会获取所有元素的内容，包括script和style标签里面的内容，innerText不会
     **区别2：**innerText不会返回隐藏元素的文本(像display:none)，但textContent会
    **区别3：**innerText会受CSS的影响，触发重排，而textContent不会，所以textContent快
但用这两个API赋值的时候，都会覆盖掉元素里原有的内容
    ```
    div1.textContent = 'hello';  //这样div1里的东西都会被hello覆盖掉
   div1.appendChild(document.createTextNode('hello'));  //这样就不会覆盖掉div1里的东西
    ```
# 3. Document API
- **1.document.children**  
[html]，document是html的爸爸

- **2.document.documentElement**  
就是html

- **3.document.write**
尽量不用，若用在延时性和异步性的方法里，会把整个页面的内容覆盖掉

- **4.querySelector()和querySelectorAll()**
querySelectorAll返回的是一个伪数组

# 4. 用JS完成页面内的跳转
```
let aTags = document.querySelectorAll('nav.menu > ul > li > a');   //获取所有a标签，返回的是一个数组
for(let i=0;i<aTags.length;i++) {     //因为aTags是数组，所以要用for循环，同时监听到数组里的所有元素
    aTags[i].onclick = function (x) { //监听a标签被鼠标点击的动作
        x.preventDefault();           //防止默认动作（跳转）
        let a = x.currentTarget;      //当前监听的元素a
        let href = a.getAttribute('href');  //获取a的href值
        let element = document.querySelector(href);  //获取带有这个href值的标签 例：'#siteAbout'
        let top = element.offsetTop; //获取这个标签的底端到整个页面顶端的距离（固定值）
        window.scrollTo(0,top);  //跳转window.scrollTo(x,y);
    }
}
```

# 5. console.log调试大法
出问题了就在每一步console.log，能解决很多问题

# 6. setInterval()
```
let i=0;
let say = setInterval(() => {
            if(i === 25){
                window.clearInterval(say);      //调用setInterval的名字来停止它
                return;
            }
            i++;
           console.log(i);    //从1报数到25，一秒一次，到25就停
        },1000);
```

# 7. Tween.js
[缓动动画速查表](http://easings.net/zh-cn)
1. 在[cdn](cdnjs.com)里搜tween，并在html中引用
2. 在[github-tween](https://github.com/tweenjs/tween.js/)中查看使用教程
 

