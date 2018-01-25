---
title: A27-DOM API
date: 2018-01-25 22:23:28
tags:
---

## DOM
- [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)
- [阮一峰](http://javascript.ruanyifeng.com/dom/node.html)
什么是DOM，DOM最重要的就是要明白它的概念，否则遇到问题也不知道如何下手
大家都说DOM树，这是一个比喻，易于理解，
D就是文档，想象它是一个树状的结构（就像之前数据结构里的二叉树那样），树上有着节点（node）
O就是对象，
M也就是模型，把D和O做一个一一对应的映射，就是这个模型
所以DOM就是把文档变成一个对象
![映射](http://upload-images.jianshu.io/upload_images/9047034-b2ba8fd51edc6b54.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**上图红字**
DOM转化来的js对象里面到底存放什么由DOM标准规定
比如：
上图的 head、body、meta、link、h1、p 都是Element的实例
（中间存在其它函数，F12调试可见，比如document.body就是document.body.__proto__->HTMLBodyElement.prototype.__proto__->HTMLElement.prototype.__proto__->Element.prototype）
根节点html比较特殊，是由document构造的（DOucment->HTMLDocument,简写）
至于文本节点则是由

## Node Api
### Node 属性
记住以下单词；
- child / children / parent
- node 
- first / last
- next / previous
- sibling / siblings
- type
- value / text / content
- inner / outer
- element
然后相互组合
一部分：childNodes,firstChild,innerText,lastChild,nextSibling,nodeName,nodeType,nodeValue,outerText,ownerDocument,parentElement,parentNode,previousSibling,textContent
```
<html>
  <head></head>
  <body></body>
</html>

document.body.previousSibling // #text
document.body.previousSibling.previousSibling // <head></head>
docuemnt.body.previousElementSibling // <head></head>
// 为什么previousSibling返回的是文本节点呢，这是因为本来dom是配对xml的，后来强行配对html
// previousSibling是 Node 的属性，previousElementSibling是 Element 的属性
// previousSibling 是本来就有的，previousElementSibling是后来加的
// 类似的还有 nextSibling / nextElementSibling, firstChild / firstElementChild 等等
// 可以在F12中尝试，控制台会显示提示以及该属性属于哪个对象
```

### 几个要注意的 Node Api
- `nodeName`
  注意这个api很奇葩
  `document.body.nodeName // 'BODY`
  `document.documentElement.nodeName // 'HTML`
  `document.nodeName不行，必须像上面那样写`
  它对所有标签返回名称都是大写
  **但是！唯有svg，它返回小写**
  `document.getElementsByTagName('svg')[0] // 'svg'`
  这是因为svg是‘外来的’标签，它是后来新加的，然后就这么小写了……
- `nodeType` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)
  只读属性 Node.nodeType 表示的是该节点的类型。其所有可能的值请参考[节点类型常量.](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType#%E8%8A%82%E7%82%B9%E7%B1%BB%E5%9E%8B%E5%B8%B8%E9%87%8F)
  它返回一个整数来表示节点的类型
  (之所以这样是因为当年的计算机还没这么发达，而1比element的字节小多了)
  以下几个需知：
  1  元素节点， Eg：<div>、<p>
  3  文本节点， 元素或者属性中实际的文字，比如<p>段落</p>中的段落二字
  9  Document节点， 
  11 DocumentFragment节点 // 很特殊的一个节点（暂时我还不清楚，google DocumentFragment优化）
  ```
  document.body.nodeType // 1
  document.nodeType // 9
  document.documentElement.nodetype // 1
  document.documentElement.nodeName // 'HTML'
  // 所以document.documentElement才是html，document不是？
  ```
- `innerText`与`textContent` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent)
  区别:
  - textContent 会获取所有元素的内容，包括 <script> 和 <style> 元素，然而 innerText 不会。
  - innerText意识到样式，并且不会返回隐藏元素的文本(设置display:none的元素)，而textContent会。
  - 由于 innerText 受 CSS 样式的影响，它会触发重排（reflow），但textContent 不会。
  - 与 textContent 不同的是, 在 Internet Explorer (对于小于等于 IE11 的版本) 中对 innerText 进行修改， 不仅会移除当前元素的子节点，而且还会永久性地破坏所有后代文本节点（所以不可能再次将节点再次插入到任何其他元素或同一元素中）。

### Node 方法 （如果一个属性是函数，那么这个属性就也叫做方法；换言之，方法是函数属性）
- `appendChild()`
- `cloneNode() // 分深、浅拷贝`
- `contains()`
- `hasChildNodes()`
- `insertBefore()`
- `isEqualNode() // 看起来相等`
- `isSameNode() // 完全相等，可以用 === 来代替`
- `removeChild()`
- `replaceChild()`
- `normalize() // 常规化`
基本看见名字就知道作用，不清楚也可以查MDN。

## Document Api -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)
### Document 属性
- body
- characterSet
- childElementCount
- children
- doctype
- documentElement
- domain
- fullscreen
- head
- hidden
- images
- links
- location
- onxxxxxxxxx
- origin
- plugins
- readyState
- referrer
- scripts
- scrollingElement
- styleSheets
- title
- visibilityState
### Document 方法：
- close()
- createDocumentFragment()
- createElement()
- createTextNode()
- execCommand()
- exitFullscreen()
- getElementById()
- getElementsByClassName()
- getElementsByName()
- getElementsByTagName()
- getSelection()
- hasFocus()
- open()
- querySelector()
- querySelectorAll()
- registerElement()
- write()
- writeln()

## Element Api -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element)

关于 DOM API 更多见之后写的常用 API。（待续）