---
title: 自己实现数组常用API
date: 2019-03-23 11:30:34
tags:
---
#### 1. join
```
Array.prototype.join = function(char){
  let result = this[0] || ''
  let length = this.length
  for(let i=1; i< length; i++){
      result += char + this[i]
  }
  return result
}
```
#### 2. slice
```
Array.prototype.slice = function(begin, end){
    let result = []
    begin = begin || 0
    end = end || this.length
    for(let i = begin; i< end; i++){
        result.push(this[i])
    }
    return result
}
```
因此可以用 slice 来将伪数组，转化成数组
```
array = Array.prototye.slice.call(arrayLike)
或者
array = [].slice.call(arrayLike)
```
但是没必要，因为ES6有更好的方法
```
array = Array.from(arrayLike)
```
P.S. 伪数组与真数组的区别就是：伪数组的原型链中没有 Array.prototype，而真数组的原型链中有 Array.prototype。因此伪数组没有 pop、join 等属性。
#### 3. sort
```
Array.prototype.sort = function(fn){
    fn = fn || (a,b)=> a-b
    let roundCount = this.length - 1 // 比较的轮数
    for(let i = 0; i < roundCount; i++){
        let minIndex = this[i]
        for(let k = i+1; k < this.length; k++){
            if( fn.call(null, this[k],this[i]) < 0 ){
                [ this[i], this[k] ] = [ this[k], this[i] ]
            }
        }
    }
}
```
#### 4. forEach
```
Array.prototype.forEach = function(fn){
    for(let i=0;i<this.length; i++){
        if(i in this){
            fn.call(undefined, this[i], i, this)
        }
    }
}
```
forEach 和 for 的区别主要有两个：
- 1.forEach 没法 break
- 2.forEach 用到了函数，所以每次迭代都会有一个新的函数作用域；而 for 循环只有一个作用域（著名前端面试题就是考察了这个）
#### 5. map
```
Array.prototype.map = function(fn){
    let result = []
    for(let i=0;i<this.length; i++){
        if(i in this) {
            result[i] = fn.call(undefined, this[i], i, this)
        }
    }
    return result
}
```
map和forEach的区别就是map有返回值而forEach没有
#### 6. filter
```
Array.prototype.filter = function(fn){
    let result = []
    let temp
    for(let i=0;i<this.length; i++){
        if(i in this) {
            if(temp = fn.call(undefined, this[i], i, this) ){  // fn.call() 返回真值就 push 到返回值，没返回真值就不 push。
                result.push(this[i])
            }
        }
    }
    return result
}
```
#### 7. reduce
```
Array.prototype.reduce = function(fn, init){
    let result = init
    for(let i=0;i<this.length; i++){
        if(i in this) {
            result = fn.call(undefined, result, this[i], i, this)
        }
    }
    return result
}
```

- 1. reduce 实现 map
```
 array2 = array.map( (v) => v+1 )
 可以写成 
 array2 = array.reduce( (result, v)=> {
     result.push(v + 1)
     return result
 }, [ ] )
 ```
- 2. reduce 实现 filter
```
 array2 = array.filter( (v) => v % 2 === 0 )
 可以写成
 array2 = array.reduce( (result, v)=> {
     if(v % 2 === 0){ result.push(v) }
     return result
 }, [])
```