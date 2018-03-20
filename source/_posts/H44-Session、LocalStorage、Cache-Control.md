---
title: H44-Session、LocalStorage、Cache-Control
date: 2018-03-19 23:26:57
tags: [HTTP,Session,Cookie,LocalStroage,SessionStorage,Cache-Control,Expires,ETag]
categories: 饥人谷
---
# Cookie 存在的问题
用户可以随意篡改 Cookie

# Session 与 Cookie 的关系
一般来说，Session 基于 Cookie 来实现。
[知乎](https://www.zhihu.com/question/19786827)
*Cookie*
本质是HTTP协议的一个内容
  1. 服务器通过 Set-Cookie 头给客户端一串字符串
  2. 客户端每次访问相同域名的网页时，必须带上这段字符串
  3. 客户端要在一段时间内保存这个Cookie
  4. Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
  5. 大小大概在 4kb 以内

*Session*（不翻译）

  1. 将 SessionID（随机数）通过 Cookie 发给客户端
  2. 客户端访问服务器时，服务器读取 SessionID
  3. 服务器有一块内存（哈希表）保存了所有 session
  4. 通过 SessionID 我们可以得到对应用户的隐私信息，如 id、email
  5. 这块内存（哈希表）就是服务器上的所有 session
缺点：用户太多的话占内存高（cookie不占内存）

## 不基于 cookie 的 session
使用查询参数和localstorage来实现

**建议：前端不建议读写cookie，那是后端的事**

# [web Storage](http://javascript.ruanyifeng.com/bom/webstorage.html)

*window.localStorage*
localStorage就是HTML5的API
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage)
  1. localStorage 跟 HTTP 无关
  2. HTTP 不会带上 LocalStorage 的值
  3. 只有相同域名的页面才能互相读取 LocalStorage （没有同源那么严格）
  4. 每个域名 localStorge 最大存储量为 5Mb 左右（每个浏览器不一样） 
  5. 常用场景：记录有没有提示过用户（没有用的信息，不能记录密码）
  6. LocalStorage 永久有效，除非用户清理

*window.sessionStorage*（会话存储）
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)
  1、2、3、4同上
  5. sessionStorage 在用户关闭页面（会话结束）后就失效
**注意：**sessionStorage和上面的session并没有关系

## 使用 localStorage
```
<script>
  var a = localStorage.getItem('a')
  if(!a){
    a = 0
  }else{
    a = parseInt(a,10) + 1
  }
  console.log('a',a)
  localStorage.setItem('a',a)
</script>
```
原本页面刷新后变量全部清掉了，但是现在a被localStorage保存下来了（在c盘的一个文件里）
这叫做持久化储存

*常用的一种方式*
```
<script>
  let already = localStorage.getItem('已经提示了')
  if(!already){
    alert('你好，我们的网站已经改版了，有了这些功能：……')
    localStorage.setItem('已经提示了',true)
  }
</script>
```
这样使用localstroge来保存一个判断，就使得不会每次进入页面都出现提示了

## cookie 与 LocalStorage 的区别
cookie 会被发送给服务器而 localStorage 不会

# [HTTP 缓存](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ)
*web 性能优化*
- [Cache-Control](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)
> 通用消息头被用于在http 请求和响应中通过指定指令来实现缓存机制。缓存指令是单向的, 这意味着在请求设置的指令，在响应中不一定包含相同的指令。

功能：让浏览器在一定时间内不访问服务器，直接用本地的硬盘或内存直接作为响应 从而提高速度
更新： 在入口处（html）把url稍微变动，和以前所有的url都不一样，那么它就不会使用缓存，浏览器就回去下载最新版
```
// nodejs
if(path === '/js/main.js'{
  ...
  response.setHeader('Cache-Control','max-age=30') // 缓存30秒
  ...
})
```
如果服务器给某个文件设置了cache-control，比如上面，
那么第一次请求后30秒内你刷新页面，请求并不会被浏览器发送到服务器
首页不要设置cache-control（最好所有html都不要设置）

## 更新缓存
将一个文件设置缓存，比如上面那个mian.js，改为一年（这很常见，比如知乎就是这么设置的）
那么如果有变动怎么办？
只有相同的url才会利用缓存，
所以，只要url有一点变化就会重新获取资源
依然是main.js,假设它有所变化
那么我们在引用mian.js的index.html
```
<script src='main.js?ver=1'></script>
```
给它添加一个查询参数，它就会自动更新为最新版本
另一种方法，使用md5

- [Expires](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expires)

>Expires 头指定了一个日期/时间， 在这个日期/时间之后，HTTP响应被认为是过时的；
无效的日期，比如 0, 代表着一个过去的事件，即该资源已经过期了。
如果还有一个 设置了 "max-age" 或者 "s-max-age" 指令的Cache-Control响应头，那么  Expires 头就会被忽略。 

expires 是以前用的，现在都用 cache-control
`response.setHeader('Ex[ores', 'Tue, 20 Mar 2018 07:13:41 GMT')`
cache-control 设置的是多少时间以后过期
expires 设置的是什么时候过期
**但是**，他使用的是本地时间，也就是说如果用户不小心将自己的本地时间设置了，或者错乱了，
那么可能就会受到影响，出现bug

 [MD5](https://www.wikiwand.com/zh-hans/MD5)
 > MD5讯息摘要演算法（英语：MD5 Message-Digest Algorithm），一种被广泛使用的密码杂凑函数，可以产生出一个128位元（16位元组）的散列值（hash value），用于确保信息传输完整一致。

[node使用md5](https://www.npmjs.com/package/md5)

- [ETag](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/ETag)

> ETag HTTP响应头是资源的特定版本的标识符。这可以让缓存更高效，并节省带宽，因为如果内容没有改变，Web服务器不需要发送完整的响应。而如果内容发生了变化，使用ETag有助于防止资源的同时更新相互覆盖（“空中碰撞”）。
如果给定URL中的资源更改，则一定要生成新的Etag值。 因此Etags类似于指纹，也可能被某些服务器用于跟踪。 比较etags能快速确定此资源是否变化，但也可能被跟踪服务器永久存留。

```
var md5 = require('md5');

if(path === '/js/main.js'{
  let string = fs.readFileSync('./js/main/js', 'utf8')
  ...
  let fileMd5 = md5(string)
  if(request.headers['if-none-match'] === fileMd5){
    // 没有响应体
    response/statusCode = 304
  } else{
    response.setHeader('ETag',fileMd5)
    // 有响应体
    response.write(steing)
  }
  response.end()
})
```
cache-control 是直接不请求
etag 是请求不下载

- [Last-Modified](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified)

>The Last-Modified  是一个响应首部，其中包含源头服务器认定的资源做出修改的日期及时间。 它通常被用作一个验证器来判断接收到的或者存储的资源是否彼此一致。由于精确度比  ETag 要低，所以这是一个备用机制。包含有  If-Modified-Since 或 If-Unmodified-Since 首部的条件请求会使用这个字段。 

# [浏览器缓存详解:expires,cache-control,last-modified,etag](http://blog.csdn.net/eroswang/article/details/8302191)