---
title: 自己封装AJAX
date: 2018-08-30 23:35:30
tags:
---
1. 用原生 JS 写出一个 AJAX 请求
```
  let request = new XMLHttpRequest()
  request.open('get', '/xxx') // 配置request
  request.send()
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){ 
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }
    }
  }
  ```
2. 自己封装一个封装一个 jQuery.ajax

jQuery.ajax(url,method,body,success, fail)

满足这种 API。
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}
  nodes.addClass = function(){}
  nodes.html = function(){}
  return nodes
}
window.$ = window.jQuery

window.jQuery.ajax = function({url, method, body, successFn, failFn, headers}){
  let request = new XMLHttpRequest()
  request.open(method, url) // 配置request
  for(let key in headers) {
    let value = headers[key]
    request.setRequestHeader(key, value)
  }
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){
      if(request.status >= 200 && request.status < 300){
        successFn.call(undefined, request.responseText)
      }else if(request.status >= 400){
        failFn.call(undefined, request)
      }
    }
  }
  request.send(body)
}

function f1(responseText){}
function f2(responseText){}

myButton.addEventListener('click', (e)=>{
  window.jQuery.ajax({
    url: '/frank',
    method: 'get',
    headers: {
      'content-type':'application/x-www-form-urlencoded',
      'frank': '18'
    },
    successFn: (x)=>{
      f1.call(undefined,x)
      f2.call(undefined,x)
    },
    failFn: (x)=>{
      console.log(x)
      console.log(x.status)
      console.log(x.responseText)
    }
  })
})
```
3. 升级刚刚的 jQuery.ajax 满足 Promise 规则
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}
  nodes.addClass = function(){}
  nodes.html = function(){}
  return nodes
}
window.$ = window.jQuery

window.Promise = function(fn){
  // ...
  return {
    then: function(){}
  }
}

window.jQuery.ajax = function({url, method, body, headers}){
  return new Promise(function(resolve, reject){
    let request = new XMLHttpRequest()
    request.open(method, url) // 配置request
    for(let key in headers) {
      let value = headers[key]
      request.setRequestHeader(key, value)
    }
    request.onreadystatechange = ()=>{
      if(request.readyState === 4){
        if(request.status >= 200 && request.status < 300){
          resolve.call(undefined, request.responseText)
        }else if(request.status >= 400){
          reject.call(undefined, request)
        }
      }
    }
    request.send(body)
  })
}

myButton.addEventListener('click', (e)=>{
  let promise = window.jQuery.ajax({
    url: '/xxx',
    method: 'get',
    headers: {
      'content-type':'application/x-www-form-urlencoded',
      'frank': '18'
    }
  })

  promise.then(
    (text)=>{console.log(text)},
    (request)=>{console.log(request)}
  )

})
```

