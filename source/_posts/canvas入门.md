---
title: canvas入门
date: 2018-09-05 20:44:48
tags:
---
1. 
```
//获取canvas标签
var canvas = document.querySelector("#canvas");  

//初始化
var context = canvas.getContext('2d');  

//画线
function drawLine(x1,y1,x2,y2) {
    context.beginPath();
    context.moveTo(x1-0,y1+29);  //起点
    context.lineWidth = 5;  //线的粗细
    context.lineTo(x2-0,y2+29);  //终点
    context.stroke();  //将各个点练成线
}

//画三角形
function drawLine(x1,y1,x2,y2) {
    context.beginPath();
    context.moveTo(x1,y1);  //起点
    context.lineTo(x2,y2);  //第二个点
    context.lineTo(x3,y3);  //第三个点
    context.stroke();  //将各个点练成线
    context.closePath();  //自动将第一个点和最后一个点连起来
}
//画圆圈
function drawTriangle(x,y,r) {
    context.beginPath();
    context.arc(x-0,y+29,r,0,Math.PI*2);  //left，top，半径， 起点（以原点为圆心从第一象限的x轴开始往y轴负方向画圈），终点
    context.fill();  //将圆圈填色
}

context.strokeStyle = 'black';  //线的颜色
context.fillStyle = 'black';  //圆圈填充的颜色
context.clearRect(x,y,30,30);  //橡皮擦功能
```
[arc的参数自己试](https://www.w3schools.com/tags/tryit.asp?filename=tryhtml5_canvas_arc)

2. 
```
function setCanvasSize() {
  //获取用户界面的宽高
  var pageWidth = document.documentElement.clientWidth;
  var pageHeight = document.documentElement.clientHeight;
  //赋给画板
  canvas.width = pageWidth;
  canvas.height = pageHeight;
}
```

3. 
```
//触屏设备
canvas.ontouchstart = function(ev){
  var x = ev.touches[0].clientX;  //获取点击的位置
  var y = ev.touches[0].clientY;
}  //监听手指点击事件
canvas.ontouchmove = function(){}  //监听手指移动事件

//非触屏设备
canvas.onmousedown = function(ev){}
canvas.onmousemove = function (ev) {}
canvas.onmouseup = function () {}
canvas.onmouseout = function () {}
```