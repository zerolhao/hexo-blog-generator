---
title: H44-Session、LocalStorage、Cache-Control
date: 2018-03-19 23:26:57
tags: [HTTP,Session,Cookie,LocalStroage,Cache-Control]
categories: 饥人谷
---
# Cookie 存在的问题
用户可以随意篡改 Cookie

# Session 与 Cookie 的关系
一般来说，Session 基于 Cookie 来实现。

Cookie
本质是HTTP协议的一个内容
  1. 服务器通过 Set-Cookie 头给客户端一串字符串
  2. 客户端每次访问相同域名的网页时，必须带上这段字符串
  3. 客户端要在一段时间内保存这个Cookie
  4. Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
  5. 大小大概在 4kb 以内

Session

  1. 将 SessionID（随机数）通过 Cookie 发给客户端
  2. 客户端访问服务器时，服务器读取 SessionID
  3. 服务器有一块内存（哈希表）保存了所有 session
  4. 通过 SessionID 我们可以得到对应用户的隐私信息，如 id、email
  5. 这块内存（哈希表）就是服务器上的所有 session
缺点：用户太多的话占内存高（cookie不占内存）

# localStorage
localStorage就是HTML5的API
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)

## 使用 localStroage
```
<script>
  var a = localStroage.getItem('a')
  if(!a){
    a = 0
  }else{
    a = parseInt(a,10) + 1
  }
  console.log('a',a)
  localStroage.setItem('a',a)
</script>
```
原本页面刷新后变量全部清掉了，但是现在a被localstroage保存下来了（在c盘的一个文件里）
这叫做持久化储存

*常用的一种方式*
```
<script>
  let already = localStroage.getItem('已经提示了')
  if(!already){
    alert('你好，我们的网站已经改版了，有了这些功能：……')
    localStroage.setItem('已经提示了',true)
  }
</script>
```
这样使用localstroge来保存一个判断，就使得不会每次进入页面都出现提示了