---
title: JavaScript实现各种排序算法
date: 2019-01-04 18:34:51
tags:
---
#### 1.冒泡排序
##### 实现方式
```
function bubleSort(arr) {
  for(let i = 0; i < arr.length /*i代表轮数*/; i++) {
    for(let j = 0; j < arr.length - i /*j代表当前轮选中的元素下标*/; j++) {
      if(arr[j] > arr[j+1]) {
        [ arr[j], arr[j+1] ] = [ arr[j+1], arr[j] ] /*交换元素*/
      }
      //console.log(arr)
    }
  }
}

var arr = [10, 34, 21, 47, 3, 28]
bubleSort(arr)
```
##### 时间复杂度
时间复杂度(可以理解为排序的次数)计算： (n-1) + (n-2) + ... + 1 = n*(1 + (n-1))/2，所以时间复杂度为 O(n^2) 
#### 2.选择排序
##### 实现方式
```
function selectSort(arr) {
  for(let min = i = 0; i < arr.length /*i代表轮数*/; i++) {
    min = i
    for(let j = i + 1; j < arr.length; j++) {
      if(arr[min] > arr[j]) {
        min = j
      }
    }
    [ arr[i], arr[min] ] = [ arr[min], arr[i] ] //把每轮的第一个和当前轮的最小值交换位置
  }
}

var arr = [10, 34, 21, 47, 3, 28]
sselectSort(arr)
```
##### 时间复杂度
时间复杂度为 O(n^2) 
#### 3.插入排序
##### 实现方式
```
 function insertSort(arr) {
   for (let i = 1; i < arr.length; i++) {
     for (let j = 0; j < i; j++) {
       if (arr[i] < arr[j]) {
         arr.splice(j, 0, arr[i])
         arr.splice(i + 1, 1)
         break
       }
     }
   }
 }

var arr = [10, 34, 21, 47, 3, 28]
insertSort(arr)
```
##### 时间复杂度
时间复杂度为 O(n^2) 
#### 4.快速排序
##### 实现方式
```
 function quickSort(arr) {
  if(arr.length <= 1) {
    return arr
  }
  let leftArr = [] 
  let rightArr = []
  for(let i = 1; i < arr.length; i++) {
    if(arr[i] >= arr[0]) {
      rightArr.push(arr[i])
    } else {
      leftArr.push(arr[i])
    }
  }
  return quickSort(leftArr).concat(arr[0]).concat(quickSort(rightArr))
}

var arr = [10, 34, 21, 47, 3, 28]
quickSort(arr)
```
##### 时间复杂度
时间复杂度为 O(nlogn)