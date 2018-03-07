---
title: A34-AJAX
date: 2018-03-07 11:16:05
tags:
---
# 如何发请求？
用 form 可以发请求，但是会刷新页面或新开页面
用 a 可以发 get 请求，但是也会刷新页面或新开页面
用 img 可以发 get 请求，但是只能以图片的形式展示
用 link 可以发 get 请求，但是只能以 CSS、favicon 的形式展示
用 script 可以发 get 请求，但是只能以脚本的形式运行

有没有什么方式可以实现
  1. get、post、put、delete 请求都行
  2. 想以什么形式展示就以什么形式展示

# 微软的突破
IE 5 率先在 JS 中引入 ActiveX 对象（API），使得 JS 可以直接发起 HTTP 请求。
随后 Mozilla、 Safari、 Opera 也跟进（抄袭）了，取名 XMLHttpRequest，并被纳入 W3C 规范

# [AJAX](http://javascript.ruanyifeng.com/bom/ajax.html)
Jesse James Garrett 讲如下技术取名叫做 AJAX：异步的 JavaScript 和 XML
Asynchronous JavaScript and XML
1. 使用 XMLHttpRequest 发请求
2. 服务器返回 XML 格式的字符串
3. JS 解析 XML，并更新局部页面
（但我们现在不同XML改用更好用的JSON了）

# XMLHttpRequest实例的属性
- [readyState](http://javascript.ruanyifeng.com/bom/ajax.html#toc2)
readyState是一个只读属性，用一个整数和对应的常量，表示XMLHttpRequest请求当前所处的状态。

> 0，对应常量UNSENT，表示XMLHttpRequest实例已经生成，但是open()方法还没有被调用。
> 1，对应常量OPENED，表示send()方法还没有被调用，仍然可以使用setRequestHeader()，设定HTTP请求的头信息。
> 2，对应常量HEADERS_RECEIVED，表示send()方法已经执行，并且头信息和状态码已经收到。
> 3，对应常量LOADING，表示正在接收服务器传来的body部分的数据，如果responseType属性是text或者空字符串，responseText就会包含已经收到的部分信息。
> 4，对应常量DONE，表示服务器数据已经完全接收，或者本次接收已经失败了。

- [readyStateChange](http://javascript.ruanyifeng.com/bom/ajax.html#toc22)
`readyState`属性的值发生改变，就会触发readyStateChange事件。
我们可以通过onReadyStateChange属性，指定这个事件的回调函数，对不同状态进行不同处理。尤其是当状态变为4的时候，表示通信成功，这时回调函数就可以处理服务器传送回来的数据。

- [onreadystatechange](http://javascript.ruanyifeng.com/bom/ajax.html#toc3)
onreadystatechange属性指向一个回调函数，当readystatechange事件发生的时候，这个回调函数就会调用，并且XMLHttpRequest实例的readyState属性也会发生变化。
另外，如果使用abort()方法，终止XMLHttpRequest请求，onreadystatechange回调函数也会被调用。

- [response](http://javascript.ruanyifeng.com/bom/ajax.html#toc4)
response属性为只读，返回接收到的数据体（即body部分）。
它的类型可以是ArrayBuffer、Blob、Document、JSON对象、或者一个字符串，这由XMLHttpRequest.responseType属性的值决定。
如果本次请求没有成功或者数据不完整，该属性就会等于null。

- [responseType](http://javascript.ruanyifeng.com/bom/ajax.html#toc5)
responseType属性用来指定服务器返回数据（xhr.response）的类型。

> ”“：字符串（默认值）
> “arraybuffer”：ArrayBuffer对象
> “blob”：Blob对象
> “document”：Document对象
> “json”：JSON对象
> “text”：字符串

text类型适合大多数情况，而且直接处理文本也比较方便，document类型适合返回XML文档的情况，blob类型适合读取二进制数据，比如图片文件。
如果将这个属性设为“json”，支持JSON的浏览器（Firefox>9，chrome>30），就会自动对返回数据调用JSON.parse()方法。也就是说，你从xhr.response属性（注意，不是xhr.responseText属性）得到的不是文本，而是一个JSON对象。

XHR2支持Ajax的返回类型为文档，即xhr.responseType=”document” 。这意味着，对于那些打开CORS的网站，我们可以直接用Ajax抓取网页，然后不用解析HTML字符串，直接对XHR回应进行DOM操作。

- [responseText](http://javascript.ruanyifeng.com/bom/ajax.html#toc6)
responseText属性返回从服务器接收到的字符串，该属性为只读。如果本次请求没有成功或者数据不完整，该属性就会等于null。
如果服务器返回的数据格式是JSON，就可以使用responseText属性。
```
var data = ajax.responseText;
data = JSON.parse(data);
```

- [status](http://javascript.ruanyifeng.com/bom/ajax.html#toc8)
status属性为只读属性，表示本次请求所得到的HTTP状态码，它是一个整数。一般来说，如果通信成功的话，这个状态码是200。

- [statusText](http://javascript.ruanyifeng.com/bom/ajax.html#toc9)
statusText属性为只读属性，返回一个字符串，表示服务器发送的状态提示。不同于status属性，该属性包含整个状态信息，比如”200 OK“。

-[open()](http://javascript.ruanyifeng.com/bom/ajax.html#toc17)
XMLHttpRequest对象的open方法用于指定发送HTTP请求的参数，它的使用格式如下，一共可以接受五个参数。
```
void open(
   string method,
   string url,
   optional boolean async,
   optional string user,
   optional string password
);
```
> method：表示HTTP动词，比如“GET”、“POST”、“PUT”和“DELETE”。
> url: 表示请求发送的网址。
> async: 格式为布尔值，默认为true，表示请求是否为异步。如果设为false，则send()方法只有等到收到服务器返回的结果，才会有返回值。
> user：表示用于认证的用户名，默认为空字符串。
> password：表示用于认证的密码，默认为空字符串。

如果对使用过open()方法的请求，再次使用这个方法，等同于调用abort()。
下面发送POST请求的例子。
```
xhr.open('POST', encodeURI('someURL'));
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
xhr.onload = function() {};
xhr.send(encodeURI('dataString'));
```
上面方法中，open方法向指定URL发出POST请求，send方法送出实际的数据。

下面是一个同步AJAX请求的例子。
```
var request = new XMLHttpRequest();
request.open('GET', '/bar/foo.txt', false);
request.send(null);

if (request.status === 200) {
  console.log(request.responseText);
}
```

- [send()](http://javascript.ruanyifeng.com/bom/ajax.html#toc18)
send方法用于实际发出HTTP请求。如果不带参数，就表示HTTP请求只包含头信息，也就是只有一个URL，典型例子就是GET请求；如果带有参数，就表示除了头信息，还带有包含具体数据的信息体，典型例子就是POST请求。
```
ajax.open('GET'
  , 'http://www.example.com/somepage.php?id=' + encodeURIComponent(id)
  , true
);

// 等同于
var data = 'id=' + encodeURIComponent(id));
ajax.open('GET', 'http://www.example.com/somepage.php', true);
ajax.send(data);
```
上面代码中，GET请求的参数，可以作为查询字符串附加在URL后面，也可以作为send方法的参数。
下面是发送POST请求的例子。
```
var data = 'email='
  + encodeURIComponent(email)
  + '&password='
  + encodeURIComponent(password);
ajax.open('POST', 'http://www.example.com/somepage.php', true);
ajax.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
ajax.send(data);
```
如果请求是异步的（默认为异步），该方法在发出请求后会立即返回。如果请求为同步，该方法只有等到收到服务器回应后，才会返回。

注意，所有XMLHttpRequest的监听事件，都必须在send()方法调用之前设定。

send方法的参数就是发送的数据。多种格式的数据，都可以作为它的参数。
```
void send();
void send(ArrayBufferView data);
void send(Blob data);
void send(Document data);
void send(String data);
void send(FormData data);
```

# JSON -- 一门新语言
[http://json.org/](http://json.org/)
它和 JavaScript的区别：
  1. 没有抄袭 function 和 undefined
  2. JSON 的字符串首尾必须是 双引号
  3. 
JS         VS       JSON
undefined           没有
null                null
['a','b']           ["a","b"]
function fn(){}     没有
{name: 'zero'}      {"name": "zero"}
'string'            "string"
var a = {}
a.self = a          搞不定（没有变量）
{__proto__}         没有原型链（只是简单的哈希）

# 如何使用 XMLHttpRequest
```
myButton.addEventListener('click', (e)=>{
  let request = new XMLHttpRequest()
  request.open('get', '/xxx') // 配置request
  request.send()
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){
      console.log('请求响应都完毕了')
      console.log(request.status)
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
        console.log(typeof request.responseText)
        console.log(request.responseText)
        let string = request.responseText
        // 把符合 JSON 语法的字符串
        // 转换成 JS 对应的值
        let object = window.JSON.parse(string) 
        // JSON.parse 是浏览器提供的
        console.log(typeof object)
        console.log(object)
        console.log('object.note')
        console.log(object.note)

      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }

    }
  }
})

// 后台
// 后端代码
  }else if(path==='/xxx'){
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/json;charset=utf-8')
    response.setHeader('Access-Control-Allow-Origin', 'http://frank.com:8001') // 允许http://frank.com:8001访问
    response.write(`
    {
      "note":{
        "to": "小谷",
        "from": "方方",
        "heading": "打招呼",
        "content": "hi"
      }
    }
    `)
    response.end()
```
[完整代码]()

# [同源策略](http://javascript.ruanyifeng.com/bom/same-origin.html)
只有 协议+端口+域名 一模一样才允许发 AJAX 请求
  1. http://baidu.com 可以向 http://www.baidu.com 发 AJAX 请求吗 no
  2. http://baidu.com:80 可以向 http://baidu.com:81 发 AJAX 请求吗 no

浏览器必须保证
只有 协议+端口+域名 一模一样才允许发 AJAX 请求
CORS 可以告诉浏览器，我俩一家的，别阻止他

突破同源策略 === 跨域

Cross-Origin Resource Sharing
跨域资源共享

## 为什么 form 可以跨域而 AJAX 不行
因为原页面用form提交到另一个域名之后，原页面的脚本无法获取新页面中的内容。
所以浏览器认为这是安全的。
而AJAX是可以读取响应内容的，因此浏览器不允许你这么做。
如果你F12查看的话，会发现请求已经发送出去了，只是你拿不到响应而已。
所以浏览器这个策略的本质是，一个域名的js，在未经允许的情况下，不得读取另一个域名的内容，但浏览器并不阻止你向另一个域名发送请求。

# [CORS 跨域](http://javascript.ruanyifeng.com/bom/cors.html)
上面例子的这句代码
服务器添加响应头
`response.setHeader('Access-Control-Allow-Origin', 'http://frank.com:8001')`
就是设置服务器允许 `http://frank.com:8001` 源访问（假设服务器是）`http://jack.com:8002`

# 调试小技巧
```
console.time()
var i = 0;
console.timeEnd()
// 会将console中间的代码执行花费的时间打印出来
```


参考文章：[阮一峰](http://javascript.ruanyifeng.com/bom/ajax.html)
