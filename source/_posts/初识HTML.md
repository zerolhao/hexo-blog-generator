---
title: 初识HTML
date: 2017-12-02 19:04:07
tags:
---
## 1. 万维网联盟（World  Wide Web Consortium, W3C）,又称 W3C 理事会，是万维网的主要国际标准组织。

- ## 历史
万维网联盟（W3C）由蒂姆·伯纳斯-李于1994年10月离开欧洲核子研究中心（CERN）后成立，在欧盟执委会和国防高等研究计划署（DARPA）的支持下成立于麻省理工学院MIT计算机科学与人工智能实验室（MIT／LCS），DARPA曾率先推出了互联网及其前身ARPANET。

- ## 标准
为解决web应用中不同平台、技术和开发者带来的不兼容问题，保障Web信息的顺利和完整流通，万维网联盟制定了一系列标准并督促Web应用开发者和内容提供者遵循这些标准。标准的内容包括使用语言的规范，开发中使用的导则和解释引擎的行为等等。W3C也制定了包括XML和CSS等的众多影响深远的标准规范。

更多详细请见：[wiki](https://www.wikiwand.com/zh-cn/%E4%B8%87%E7%BB%B4%E7%BD%91%E8%81%94%E7%9B%9F)
[W3C官网](https://www.w3.org/)

## 2. MDN Web Docs（旧称Mozilla Developer Network、Mozilla Developer Center，简称MDN）是一个汇集众多Mozilla基金会产品和网路技术开发文档的免费网站。

- ## 历史
该计画始于2005年，最初由Mozilla公司员工Deb Richardson 领导。自2006年以来，文档工作由Eric Shepherd领导。

网站最初的内容是由DevEdge提供，但在AOL收购Netscape后，DevEdge网站也宣布关闭。为此Mozilla基金会向AOL取得了DevEdge释出的内容，同时将DevEdge内容搬移到mozilla.org。

MDN本身有一个论坛，并在Mozilla IRC 网络上有一个 IRC 频道#mdn。MDN由Mozilla公司提供服务器和员工的资助。

2016年10月3日发表的Brave网页浏览器将MDN作为其搜寻引擎选项之一。

更多详细请见：[wiki](https://www.wikiwand.com/zh-hans/MDN_Web_Docs)
[MDN官网](https://developer.mozilla.org/zh-CN/)

## 3. HTML元素
以下是一部分html元素，考虑到时间我并没有写这些元素各自所代表的含义（其实就是懒，但是真的太麻烦啦！），如果你想要明白它们所代表的含义的话，就去MDN看吧！[MDN-HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)
```
<a>
<abbr>
<acronym>
<address>
<applet>
<area>
<article>
<aside>
<audio>
<b>
<base>
<basefont>
<bdi>
<bdo>
<bgsound>
<big>
<blink>
<body>
<br>
<button>
<canvas>
<caption>
<center>
<cite>
<code>
<col>
<colgroup>
<content>
<data>
<dd>
<del>
<details>
<dfn>
<dialog>
<dir>
<div>
<dl>
<dt>
<element>
<em>
<embed>
<fieldset>
<figcaption>
<figure>
<font>
<footer>
<form>
<frame>
<frameset>
<head>
<header>
<hgroup>
<hr>
<html>
<i>
<iframe>
<image>
<img>
<input>
<ins>
<isindex>
<kbd>
<keygen>
<label>
<legend>
<li>
<link>
<listing>
<main>
<map>
<mark>
<marquee>
<menu>
<menuitem>
<meta>
<meter>
<multicol>
<nav>
<nextid>
<nobr>
<noembed>
<noframes>
<noscript>
<object>
<ol>
<optgroup>
<option>
<output>
<p>
<param>
<picture>
<plaintext>
<pre>
<q>
<rp>
<rt>
<rtc>
<ruby>
<s>
<samp>
<script>
<section>
<select>
<shadow>
<slot>
<small>
Source
<spacer>
<span>
<strike>
<strong>
<style>
<sub>
<summary>
<sup>
<table>
<tbody>
<td>
<template>
<textarea>
<tfoot>
<th>
<thead>
<time>
<title>
<tr>
<track>
<tt>
<u>
<ul>
<var>
<video>
<wbr>
<xmp>
```

## 4.空标签
虽然不能一个一个解释下每个标签的含义，但是可以解释一些特别的东西。
空标签就是这种“特别”之一。
正常情况下，一个 HTML 元素通常由一个**开始**标记和**结束**标记组成，其内容插入在：
`<tagname> 内容xxx</tagname>`
例如：`<p>A Paragraph</p>`
但是空标签不同，它**没有内容和结束标记!**
在HTML中，对一个空标签使用一个闭标签通常时无效的。
例如：`<input type="text"></inut>`
正确的写法是：`<input type="text">`
当然你也可以这么写：`<input type="text" />`（HTML5不需要关闭空元素但是你可以这么做，如果你需要一个更严格的验证的话）
在HTML中还有以下这些空元素：
```
<area>
<base>
<br>
<col>
<colgroup> when the span is present
<command>
<embed>
<hr>
<img>
<input>
<keygen>
<link>
<meta>
<param>
<source>
<track>
<wbr>
```
[详见此](https://developer.mozilla.org/zh-CN/docs/Glossary/%E7%A9%BA%E5%85%83%E7%B4%A0)

## 5. 可替换标签
从元素本身的特点来讲，HTML标签元素可以分为可替换元素(replaceable element)和不可替换元素(none-replaceable element)。
不可替换元素：
HTML中的大多数元素是不可替换元素，即其内容直接表现给用户端（例如浏览器）。
> 例如：```<p>段落的内容</p>```
> 段落```<p>```是一个不可替换元素，文字“段落的内容”全被显示。

**可替换元素:**
可替换元素就是浏览器根据元素的标签和属性，来决定元素的具体显示内容。
> 例如浏览器会根据```<img>```标签的src属性的值来读取图片信息并显示出来，而如果查看(x)html代码，
> 则看不到图片的实际内容；又例如根据```<input>```标签的type属性来决定是显示输入框，还是单选按钮等。

更详细的解释(从[CSS2.1规范](https://www.w3.org/TR/CSS21/conform.html))：
> 内容超出CSS格式模型范围的元素，如图像，嵌入式文档或小程序。
> 例如，HTML IMG元素的内容通常被其“src”属性指定的图像取代。
> 替换的元素通常具有固有的尺寸：固有宽度，固有高度和固有比率。
> 例如，位图图像具有以绝对单位指定的固有宽度和固有高度（从中可明显确定固有比率）。
> 另一方面，其他文档可能没有任何固有的尺寸（例如，空白的HTML文档）。
> 如果认为这些维度可能会泄露敏感信息给第三方，用户代理可能会认为被替换的元素没有任何内在维度。
> 例如，如果一个HTML文档根据用户的银行余额改变了内在大小，那么UA可能希望这个资源没有固有维度。
> CSS渲染模型中不考虑替换元素的内容。

常见的可替换元素有：
```
<img>
<object>
<video>
<textarea>
<input>
<audio>
<canvas>
<select>
```

