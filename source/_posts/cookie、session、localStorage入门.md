---
title: cookie、 session、localStorage入门
date: 2018-08-31 00:37:08
tags:
---
# 1. cookie和session的区别
- 一般来说，Session 基于 Cookie 来实现。
### 1. cookie
1. 服务器通过 Set-Cookie 头给客户端一串字符串
2.  客户端每次访问相同域名的网页时，必须带上这段字符串
3.  客户端要在一段时间内保存这个Cookie
4.  Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
5. 大小大概在 4kb 以内
### 2. session
1. 将 SessionID（随机数）通过 Cookie 发给客户端
2. 客户端访问服务器时，服务器读取 SessionID
3. 服务器有一块内存（哈希表）保存了所有 session
4. 通过 SessionID 我们可以得到对应用户的隐私信息，如 id、email
5. 这块内存（哈希表）就是服务器上的所有 session
# 2. localStorage
1. LocalStorage 跟 HTTP 无关
2. HTTP 不会带上 LocalStorage 的值
3. 只有相同域名的页面才能互相读取 LocalStorage（没有同源那么严格）
4. 每个域名 localStorage 最大存储量为 5Mb 左右（每个浏览器不一样）
5. 常用场景：记录有没有提示过用户（没有用的信息，不能记录密码）
6. LocalStorage 永久有效，除非用户清理缓存
# 3. SessionStorage（会话存储）
- 1、2、3、4 同上
5. SessionStorage 在用户关闭页面（会话结束）后就失效。
