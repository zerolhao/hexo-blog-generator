---
title: A33-JSONP
date: 2018-03-05 21:13:53
tags:
---
# 数据库是什么
1. 文件系统是一种数据库
2. MySQL 是一种数据库
只要能长久的存数据，就是数据库

# 用数据库做加减法（更改用户账户余额）
[https://github.com/zerolhao/node-demo/tree/master/A33-lessonCode](https://github.com/zerolhao/node-demo/tree/master/A33-lessonCode)

# 局部刷新怎么做
不返回html，返回js

方案一：用图片造get请求
```
button.addEventListener('click', (e)=>{
    let image = document.createElement('img')
    image.src = '/pay'
    image.onload = function(){ // 状态码是 200~299 则表示成功
        alert('成功')
    }
    image.onload = function(){ // 状态码大于等于 400 则表示失败
        alert('失败')
    }
})
```
方案二：用script造get请求
```
button.addEventListener('click', (e)=>{
    let script = document.createElement('script')
    script.src = '/pay'
    document.body.appendChild(script) // 和图片不同，必须添加到页面内才会执行
    script.onload = function(e){ // 状态码是 200~299 则表示成功
      // 删除script标签，如果想看见可以 debugger
      e.currentTarget.remove()
    }
    script.onload = function(e){ // 状态码大于等于 400 则表示失败
      alert('fail')
      e.currentTarget.remove()
    }
})
// 后端代码
if (path === '/pay'){
    let amount = fs.readFileSync('./db', 'utf8')
    amount -= 1
    fs.writeFileSync('./db', amount)
    response.setHeader('Content-Type', 'application/javascript')
    response.write('amount.innerText = ' + amount) // 浏览器执行此代码以更新页面‘余额’
    // 也可以使用es6：response.write(`amount.innerText = amount.innerText - 1`)
    response.end()
}
```
这种技术叫做 SRJ - Server Rendered JavaScript
## 特点：域名无所谓
跨域 SRJ

# JSONP
请求方：frank.com 的前端程序员（浏览器）
响应方：jack.com 的后端程序员（服务器）
  1. 请求方创建 script，src 指向响应方，同时传一个查询参数 ?callbackName=yyy
  2. 响应方根据查询参数callbackName，构造形如
    - yyy.call(undefined, '你要的数据')
    - yyy('你要的数据')
      这样的响应
  3. 浏览器接收到响应，就会执行 yyy.call(undefined, '你要的数据')
  4. 那么请求方就知道了他要的数据
这就是 JSONP

方案3：JSONP
```
// 前端
button.addEventListener('click', (e)=>{
    let script = document.createElement('script')
    let functionName = 'frank'+ parseInt(Math.random()*10000000 ,10)
    window[functionName] = function(){  // 每次请求之前搞出一个随机的函数
        amount.innerText = amount.innerText - 0 - 1
    }
    script.src = '/pay?callback=' + functionName
    document.body.appendChild(script)
    script.onload = function(e){ // 状态码是 200~299 则表示成功
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
    script.onload = function(e){ // 状态码大于等于 400 则表示失败
        e.currentTarget.remove()
        delete window[functionName] // 请求完了就干掉这个随机函数
    }
})

// 后端
if (path === '/pay'){
    let amount = fs.readFileSync('./db', 'utf8')
    amount -= 1
    fs.writeFileSync('./db', amount)
    let callbackName = query.callback
    response.setHeader('Content-Type', 'application/javascript')
    response.write(`
        ${callbackName}.call(undefined, 'success')
    `)
    response.end()
}
```
## 约定
1. callbackName 叫做 callback
2. yyy 一般用 随机数 frank12312312312321325() // 因为可能同时调用很多网站jsonp，不如用过就没了
3. 使用 jQuery来写  
  ```
   $.ajax({ // 和ajax没关系
   url: "http://jack.com:8002/pay", // callback和函数名jq都会帮你写好
   dataType: "jsonp",
   success: function( response ) {
       if(response === 'success'){
       amount.innerText = amount.innerText - 1
       }
   }
   })
  
   $.jsonp()
  ```
# 面试题
请问 JSONP 为什么不支持 POST？
答：
  1. 因为 JSONP 是通过动态创建 script 实现的
  2. 动态创建 script 的时候只能用 get 不能用 post
  3. 
  console.log('pathWithQuery', 'queryString', 'path', 'query', 'method')
  console.log(pathWithQuery, queryString, path, query, method)

参考文章：[阮一峰](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)