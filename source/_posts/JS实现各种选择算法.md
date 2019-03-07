---
title: JavaScript实现各种选择算法
date: 2019-03-07 20:34:15
tags:
---
#### 1.线性搜索(顺序搜索)
假设一个数组中有n个元素，最好的情况就是要寻找的特定值就是数组里的第一个元素，这样仅需要1次比较就可以。而最坏的情况是要寻找的特定值不在这个数组或者是数组里的最后一个元素，这就需要进行n次比较。

+ 实现方式
```
function linearSearch(arr, value) {
  for(let i = 0; i < arr.length; i++) {
    if(arr[i] === value) {
      return i
    }
  }
  return -1
}

let arr = [9, 1, -3, 4, 2, 7]
let index = linearSearch(arr, 2)
```
##### 线性搜索优化
现实中很多查找遵循2-8原则，查找也类似。即我们80%的场景下查找的其实只是全部数据种20%的数据。根据这个原则，我们在每次查找时把找到的元素向前移动一位，这样经过多次查找后，最常查询的数据会移动到最前面，顺序查找时更容易找到。
+ 实现方式
```
function linearSearch(arr, value) {
  for(let i = 0; i < arr.length; i++) {
    if(arr[i] === value) {
      if(i > 0 ) {
        [arr[i-1], arr[i]] = [arr[i], arr[i-1]]  //让第 i 项和第 i-1 项交换位置
      }
      return i
    }
  }
  return -1
}

let arr = [9, 1, -3, 4, 2, 7]
let index = linearSearch(arr, 5)
```


