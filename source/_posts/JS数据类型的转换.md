---
title: JS数据类型的转换
date: 2018-08-07 14:06:01
tags:
---
## 1. 转换成number
#### 官方：
```
Number('1')       //     1

parseInt('1')     //     1  
parseInt是面试重点  parseInt('011') === 11，这是十进制
要在后面加8才是八进制 parseInt('011',8) === 9
parseInt('s') === NaN
parseInt('1s') === 1

parseFloat('1.23')//     1.23
```

#### 老司机：
```
'1' - 0 === 1              //最常见
'1.23' - 0 === 1.23        //最常见

+'1' === 1
+'1.23' === 1.23
+'.2' === 0.2
+'-1' === -1

-'-1' === 1
```

## 2. 转换成string
#### 官方：
1. number、boolean类型可通过toString方法转换成字符串
2. symbol不能用toString方法
3. undefined、null不能用toString方法，会报错 
4. object类型用toString只能转换出`'[object Object]'`
5. 把上述toString()改成String()也一样，但String()能转换null和undefined
```
(1).toString();        //     '1'
true.toString();       //     'true'
null.toString();       //      error:Cannot read property 'toString' of null
undefined.toString();  //      error:Cannot read property 'toString' of undefined
```

#### 老司机：
```
1 + ''       // '1'
true + ''       // 'true'
null + ''       // 'null'
undefined + ''       // 'undefined'

var obj = {};
obj + ''       // '[object Object]'

1 + '1' 等于 (1).toString + '1'      //   '11'
```

## 3. 转换成boolean
#### 官方：
总共只有6个falsy值，0、NaN、''、null、undefined，**除了这6个，其它值都是true。所有对象都是true，包括var n = new Boolean(false)，n也是true**
```
Boolean(1)          //        true
Boolean(2)          //        true
Boolean(0)          //        false
Boolean(NaN)        //        false

Boolean('')         //        空字符串false
Boolean(' ')        //        空格字符串true

Boolean(null)       //        false

Boolean(undefined)  //        false

Boolean({})         //        空对象true
Boolean([])         //        空数组true
```
#### 老司机：
取反两次，总共只有6个falsy值，0、NaN、''、null、undefined，**除了这6个，其它值都是true。所有对象都是true，包括var n = new Boolean(false)，n也是true**
```
!!1          //        true
!!0          //        false
!!NaN        //        false

!!''         //        false
!!' '        //        true

!!{}         //        true

!!null       //        false

!!undefined  //        false
```