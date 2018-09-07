---
title: let相关面试题
date: 2018-09-07 23:03:17
tags:
---
经典面试题
```
for ( var i=0; i<5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```
这里因为 var会变量提升，所以实际上是下面这样子的
```
var i
for ( i=0; i<5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```
这样一来循环一直都只有一个i，而setTimeout是个异步函数，所以会先所有会先把循环全部执行完毕，这时候 i 就是 5 了，而每次循环都会设置一个闹钟，闹钟执行时间分别是0s、1s、2s、3s、4s、5s，每一个闹钟的i都是最终的i，也就是i等于5，所以这题最终结果是输出5个5。
- 解决办法有三种
1. 使用闭包
```
for (var i = 0; i < 5; i++) {
  (function(j) {
    setTimeout(function timer() {
      console.log(j);
    }, j * 1000);
  })(i);
}
````
2. 使用 setTimeout 的第三个参数
```
for ( var i=0; i<5; i++) {
	setTimeout( function timer(j) {
		console.log( j );
	}, i*1000, i);
}
```
3. 使用 let 定义 i 
```
for ( let i=0; i<5; i++) {
	setTimeout( function timer() {
		console.log( i );
	}, i*1000 );
}
```
以上三种方法都能解决因为var变量提升而引起的误解，所以最终结果都是0s输出0，1s输出1，2s输出2，3s输出3，4s输出4，5s输出5。
