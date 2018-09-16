---
title: VueRouter入门
date: 2018-09-14 21:09:07
tags:
---
### 1.简介
由于Vue在开发时对路由支持的不足，后来官方补充了vue-router插件，它在Vue的生态环境中非常重要，在实际开发中只要编写一个页面就会操作vue-router。要学习vue-router就要先知道这里的路由是什么？这里的路由并不是指我们平时所说的硬件路由器，这里的路由就是SPA（单页应用）的路径管理器。再通俗的说，vue-router就是我们WebApp的链接路径管理系统。
有的小伙伴会有疑虑，为什么我们不能像原来一样直接用```<a></a>```标签编写链接哪？因为我们用Vue作的都是单页应用，就相当于只有一个主的index.html页面，所以你写的```<a></a>```标签是不起作用的，你必须使用vue-router来进行管理。
### 2.解读router/index.js文件
我们用vue-cli生产了我们的项目结构，你可以在src/router/index.js文件，这个文件就是路由的核心文件，我们先解读一下它。
```
import Vue from 'vue'   //引入Vue
import Router from 'vue-router'  //引入vue-router
import Hello from '@/components/Hello'  //引入根目录下的Hello.vue组件
 
Vue.use(Router)  //Vue全局使用Router
 
export default new Router({
  routes: [              //配置路由，这里是个数组
    {                    //每一个链接都是一个对象
      path: '/',         //链接路径
      name: 'Hello',     //路由名称，
      component: Hello   //对应的组件模板
    }
  ]
})
```
### 3.增加一个Hi的路由和页面
对路由的核心文件熟悉后，我们试着增加一个路由配置，我们希望在地址栏输入 http://localhost:8080/#/hi 的时候出现一个新的页面，先来看一下我们希望得到的效果。
具体操作步骤如下：
- 在src/components目录下，新建 Hi.vue 文件。
- 编写文件内容，和我们之前讲过的一样，文件要包括三个部分```<template><script>和<style>```。文件很简单，只是打印一句话。
```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>
 
<script>
export default {
  name: 'hi',
  data () {
    return {
      msg: 'Hi, I am JSPang'
    }
  }
}
</script>
 
 
<style scoped>
 
</style>
```
- 引入 Hi组件：我们在router/index.js文件的上边引入Hi组件 ```import Hi from '@/components/Hi'```
- 增加路由配置：在router/index.js文件的routes[]数组中，新增加一个对象，代码如下。
```
{
  path:'/hi',
  name:'Hi',
  component:Hi
}
```
### 4.router-link制作导航
现在通过在地址栏改变字符串地址，已经可以实现页面内容的变化了。这并不满足需求，我们需要的是在页面上有个像样的导航链接，我们只要点击就可以实现页面内容的变化。制作链接需要```<router-link>```标签，我们先来看一下它的语法。
```<router-link to="/">[显示字段]</router-link>```
#### 1.改造App.vue的导航代码
App.vue代码，注意```<route-view>```的用法。
```
<template>
  <div id="app">
    <img src="./assets/logo.png">
      <p>导航 ：
      <router-link to="/">首页</router-link> | 
      <router-link to="/hi">Hi页面</router-link> |
      <router-link to="/hi/hi1">-Hi页面1</router-link> |
      <router-link to="/hi/hi2">-Hi页面2</router-link>
      <router-link to="/hi1">Hi页面</router-link> 
     </p>
    <router-view/>
  </div>

</template>

<script>
export default {
  name: "App"
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```
#### 2.改写components/Hi.vue页面
把Hi.vue改成一个通用的模板，加入标签，给子模板提供插入位置。“Hi页面1” 和 “Hi页面2” 都相当于“Hi页面”的子页面，有点想继承关系。我们在“Hi页面”里加入标签。
- Hi1.vue
```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>
<script>
export default {
  name: 'hi',
  data () {
    return {
      msg: 'Hi, I am Hi1!'
    }
  }
}
</script>
<style scoped>
 
</style>
```
- Hi2.vue
```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>
<script>
export default {
  name: 'hi',
  data () {
    return {
      msg: 'Hi, I am Hi2'
    }
  }
}
</script>
<style scoped>
</style>
```
#### 3.Hi.vue代码
注意新增的router-view
```
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <router-view class="aaa"></router-view>
  </div>
   
</template>
 
<script>
export default {
  name: 'hi',
  data () {
    return {
      msg: 'Hi, I am JSPang'
    }
  }
}
</script>
 
 
<style scoped>
 
</style>
```
#### 4.修改router/index.js代码
我们现在导航有了，母模板和子模板也有了，只要改变我们的路由配置文件就可以了。子路由的写法是在原有的路由配置下加入children字段。
```
children:[
{path:'/',component:xxx},
{path:'xx',component:xxx},
]
```
children字段后边跟的是个数组，数组里和其他配置路由基本相同，需要配置path和component。具体看一下这个子路由的配置写法。
```
import Vue from 'vue'   
import Router from 'vue-router'  
import Hello from '@/components/Hello'  
import Hi from '@/components/Hi' 
import Hi1 from '@/components/Hi1' 
import Hi2 from '@/components/Hi2' 
 
Vue.use(Router) 
 
export default new Router({
  routes: [             
    {                    
      path: '/',        
      name: 'Hello',     
      component: Hello   
    },{
      path:'/hi',
      component:Hi,
      children:[
        {path:'/',component:Hi},
        {path:'hi1',component:Hi1},
        {path:'hi2',component:Hi2},
      ]
    }
  ]
})
```
