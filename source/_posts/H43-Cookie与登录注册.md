---
title: H43-Cookie与登录注册
date: 2018-03-17 23:11:19
tags: [HTTP,nodejs]
categories: 饥人谷
---

# 登录与注册
[代码](https://github.com/zerolhao/node-demo/tree/master/H43-Cookie%E4%B8%8E%E7%99%BB%E5%BD%95%E6%B3%A8%E5%86%8C)

# Cookie 的特点

  1. 服务器通过 Set-Cookie 响应头设置 Cookie
  2. 浏览器得到 Cookie 之后，每次请求都要带上 Cookie
  3. 服务器读取 Cookie 就知道登录用户的信息（email）
[wiki](https://www.wikiwand.com/zh/Cookie)
[方应杭](https://zhuanlan.zhihu.com/p/22396872?refer=study-fe)

## 问题

1. 我在 Chrome 登录了得到 Cookie，用 Safari 访问，Safari 会带上 Cookie 吗
  no
2. Cookie 存在哪
  Windows 存在 C 盘的一个文件里
3. Cookie会被用户篡改吗？
  可以，下节课会讲 Session 来解决这个问题，防止用户篡改
4. Cookie 有效期吗？
  默认有效期20分钟左右，不同浏览器策略不同
  后端可以强制设置有效期，具体语法看 MDN
5. Cookie 遵守同源策略吗？
  也有，不过跟 AJAX 的同源策略稍微有些不同。
  当请求 qq.com 下的资源时，浏览器会默认带上 qq.com 对应的 Cookie，不会带上 baidu.com 对应的 Cookie
  当请求 v.qq.com 下的资源时，浏览器不仅会带上 v.qq.com 的Cookie，还会带上 qq.com 的 Cookie
  另外 Cookie 还可以根据路径做限制，请自行了解，这个功能用得比较少。


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