---
title: H43-Cookie与登录注册
date: 2018-03-17 23:11:19
tags: [HTTP,nodejs]
categories: 饥人谷
---

# Cookie 的特点

  1. 服务器通过 Set-Cookie 响应头设置 Cookie
  2. 浏览器得到 Cookie 之后，每次请求都要带上 Cookie
  3. 服务器读取 Cookie 就知道登录用户的信息（email）
[wiki](https://www.wikiwand.com/zh/Cookie)
[方应杭](https://zhuanlan.zhihu.com/p/22396872?refer=study-fe)

# API
**jQuery**
- [.find()](https://www.jquery123.com/find/)
- [.val()](https://www.jquery123.com/val/)
- [$.ajax()](https://www.jquery123.com/jQuery.ajax/)
- [$.post()](https://www.jquery123.com/jQuery.post/)
- [.each()](https://www.jquery123.com/each/)
**JavaScript**
- [decodeURIComponent()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent)
- [try...catch](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch)
  **JSON**
  - [JSON.stringify()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
**Node.js**
- [readFileSync()](http://javascript.ruanyifeng.com/nodejs/fs.html#toc0)
- [writeFileSync()](http://javascript.ruanyifeng.com/nodejs/fs.html#toc1)