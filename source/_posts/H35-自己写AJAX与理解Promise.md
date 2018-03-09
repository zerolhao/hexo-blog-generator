---
title: H35-自己写AJAX与理解Promise
date: 2018-03-07 23:56:22
tags:
---
# AJAX 的所有功能
- 客户端的JS发起请求（浏览器上的）
- 服务端的JS发送响应（Node.js上的）

## 1. JS 可以设置任意请求 header 吗？
  第一部分 request.open('get', '/xxx')
  第二部分 request.setHeader('content-type','x-www-form-urlencoded')
  第四部分 request.send('a=1&b=2')
- [open()](http://javascript.ruanyifeng.com/bom/ajax.html#toc17)
- [setRequestHeader()](http://javascript.ruanyifeng.com/bom/ajax.html#toc19)
setRequestHeader方法用于设置HTTP头信息。
该方法必须在open()之后、send()之前调用。
如果该方法多次调用，设定同一个字段，则每一次调用的值会被合并成一个单一的值发送。
```
xhr.setRequestHeader('Content-Type', 'application/json');
xhr.setRequestHeader('Content-Length', JSON.stringify(data).length);
xhr.send(JSON.stringify(data));
```
上面代码首先设置头信息Content-Type，表示发送JSON格式的数据；然后设置Content-Length，表示数据长度；最后发送JSON数据。
- [send()](http://javascript.ruanyifeng.com/bom/ajax.html#toc18)

## 2. JS 可以获取任意响应 header 吗？
  第一部分 request.status / request.statusText
  第二部分 request.getResponseHeader() / request.getAllResponseHeaders()
  第四部分 request.responseText
- [status](http://javascript.ruanyifeng.com/bom/ajax.html#toc8)
`status`属性为只读属性，表示本次请求所得到的HTTP状态码，它是一个整数。一般来说，如果通信成功的话，这个状态码是200。
- [statusText](http://javascript.ruanyifeng.com/bom/ajax.html#toc9)
`statusText`属性为只读属性，返回一个字符串，表示服务器发送的状态提示。不同于status属性，该属性包含整个状态信息，比如”200 OK“。(测试显示只有 OK)
- [getAllResponseHeaders()](http://javascript.ruanyifeng.com/bom/ajax.html#toc16)
`getAllResponseHeaders`方法返回服务器发来的所有HTTP头信息。
格式为字符串，每个头信息之间使用`CRLF`(回车换行)分隔，如果没有受到服务器回应，该属性返回null。
- [getResponseHeader()](http://javascript.ruanyifeng.com/bom/ajax.html#toc16)
getResponseHeader方法返回HTTP头信息指定字段的值，如果还没有收到服务器回应或者指定字段不存在，则该属性为null。
`consolo.log(request.getResponseHeader('Content-Type'))`
- [responseText](http://javascript.ruanyifeng.com/bom/ajax.html#toc6)
responseText属性返回从服务器接收到的字符串，该属性为只读。如果本次请求没有成功或者数据不完整，该属性就会等于null。

# 还记得之前写的 window.jQuery 吗
```
window.jQuery = function(node){
    let nodes = {
        0: node,
        length: 1
    }
    return {
        addClass: function(){

        }
    }
}
```
# 今天写 window.jQuery.ajax
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}
  nodes.addClass = function(){}
  nodes.html = function(){}
  return nodes
}
window.$ = window.jQuery

/*
window.jQuery.ajax = function(options){
let url = options.url
let method = options.method
...
// es6-解构赋值
let {url, method, body, successFn, failFn, headers} = options
*/

window.jQuery.ajax = function({url, method, body, successFn, failFn, headers}){
                  // 这里直接从第一个参数开始解构，同时声明这6个变量，用let
  let request = new XMLHttpRequest()
  request.open(method, url) // 配置request
  for(let key in headers) { // 如果设置不止一个请求头
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
  window.jQuery.ajax({ // 传入对象
    url: '/frank',
    method: 'get',
    headers: {
      'content-type':'application/x-www-form-urlencoded',
      'frank': '18'
    },
    successFn: (x)=>{ // 回调函数
      f1.call(undefined,x) //同时调用复数函数
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
## ES6-解构赋值
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
[阮一峰](http://es6.ruanyifeng.com/#docs/destructuring)
```
// eg
var a = 'a', b='b',temp
temp = a
a = b // 'b'
b = temp // 'a'

// es6
var a = 'a'
var b = 'b'; // 这里必须有分号
[a,b] = [b,a]
a // 'b'
b // 'a'
```
## 回调函数
[wiki](https://www.wikiwand.com/zh-hans/%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)
> 在计算机编程中，一个回调是对一段可执行代码的引用，该代码作为参数传递给其他代码。
[jQuery文档](http://learn.jquery.com/about-jquery/how-jquery-works/#Callback_and_Functions)
> 回调是一个函数，它作为参数传递给另一个函数，并在父函数完成后执行。
> 回调的特别之处在于，在“父”节点之后出现的函数可以在回调执行之前执行。
> 另一个需要知道的重要事情是如何正确地传递回调。这就是我经常忘记正确语法的地方。
[百科](https://baike.baidu.com/item/%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)
> 回调函数就是一个通过函数指针调用的函数。
> 如果你把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用为调用它所指向的函数时，我们就说这是回调函数。
> 回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

因此，回调本质上是一种设计模式，并且jQuery(包括其他框架)的设计原则遵循了这个模式。

在JavaScript中，回调函数具体的定义为：函数A作为参数(函数引用)传递到另一个函数B中，并且这个函数B执行函数A。我们就说函数A叫做回调函数。如果没有名称(函数表达式)，就叫做匿名回调函数。

因此callback 不一定用于异步，一般同步(阻塞)的场景下也经常用到回调，比如要求执行某些操作后执行回调函数。

[参考](https://segmentfault.com/q/1010000000140970)

### 回调的问题
问题是每个程序员的回调名不一样

# Promise 解决了这个问题
基本逻辑
```
function xxx(){
    return new Promise((f1, f2) => {
        doSomething()
        setTimeout(()=>{
            // 成功就调用 f1，失败就调用 f2
        },3000)
    })
}

xxx().then(success, fail)

// 链式操作
xxx().then(success, fail).then(success, fail)
```
上面的例子改用 Promise
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}
  nodes.addClass = function(){}
  nodes.html = function(){}
  return nodes
}
window.$ = window.jQuery

/* Promise基本逻辑
//注意Promise是 window 的API
window.Promise = function(fn){
  // ...
  return {
    then: function(){}
  }
}
*/
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
  }).then( // then会返回一个Promise对象，以供调用，进行链式操作
    (text)=>{console.log(text)}, //成功就调用这个函数
    (request)=>{console.log(request)} //失败就调用这个函数
  ).then(
    (text)=>{console.log('success')}, //上面then里面代码成功了会继续调用这个函数，不管它执行的是它的第一个还是第二个参数（函数）
    (request)=>{console.log('fail')} //上面失败了会继续调用这个函数
  )
  // 等于下面,上面其实就是jq的链式操作
  promise.then(
    (text)=>{console.log(text)},
    (request)=>{console.log(request)}
  )
  Promise.then(
    (text)=>{console.log('success')},
    (request)=>{console.log('fail')}
  )

})
```
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
[阮一峰](http://es6.ruanyifeng.com/#docs/promise)