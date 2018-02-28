---
title: A29-用jQuery做个轮播
date: 2018-01-27 23:12:29
tags:
---

# 这次我们将用jQuery做一个轮播
在遵循原则：**内容(HTML)、样式(CSS)、行为(JavaScript)分离**的情况下
## 面试套路
如果被问到你对于前端内容、样式、行为分离的理解。
我们当然认为这是理所当然的，但是这么回答显然不行。
所以，**我们可以反着回答**，如果不分离的话会有什么坏处，以此论证需要分离。
例如：
- HTML负责样式
  ```
  <body bgcolor="gray">
    <center>
      <font color="red" size="20">你好</font>
    <center>
  </body>
  ```
  现在，有的标签是为了表示样式的，有的标签是为了表示内容的，这使得我们难以区分这些标签真正的逻辑结构
- CSS 表示内容
  ```
  // html
  <div id="x"></div>
  // css
  div::after{ content: '你好'; }
  // js
  console.log(x.innerText) // 什么都没有，空的
  ```
  使用css表示内容，会导致用户无法用鼠标选中它，同时，js也无法获取它的内容
- CSS 表示行为
  使用css表示复杂的行为，会导致页面变得很慢很慢
  优化的原则之一：不要使用IE发明的CSS Expressions
- JS 表示 CSS
  ```
  // html
  <div id="x"></div>
  // js
  var $div = $('#x')
  $div.hide() // 影藏很好理解，display:none
  $div.show() // 显示呢？block？可如果我本来是flex呢？或者inline-flex呢？
  // jQuery 为了处理这个问题，在 hide() 时获取了你的 display 值
  // 使得 flex 在经过 hide() show()之后依然是 flex
  // 可是，如果本来是 none 呢？
  // css
  div{ display:none; }
  // 测试结果显示 show() 之后是 block,也就是说 show() 默认 block
  ```
  这种结果显然不符合我们的预期，这里，建议使用添加 class 的方式来控制，js 只负责行为，它不知道一个div应该怎么show，block？flex？
  显示还是不显示？那是 css 的活。
  `$div.addClass('active')`

# 思路
1. 将图片全部放到一个div里，把图片放平，图片最好是一样大小的
2. 然后用一个窗口来看图片，图片从左到右挨个出现，就好像以前的电影胶带
3. 有了思路，剩下的就是实现和改bug，这次是用jq来写，会出现很多不认识的api，直接查文档就行了

# 关于封装
1. 从 API 开始思考
2. 尽量能让人看见就明白是干嘛的

# 成果
- [预览](https://zerolhao.github.io/task-code/A29-jQuery-carousel/index.html)
- [代码](https://github.com/zerolhao/task-code/tree/master/A29-jQuery-carousel)

# 本次任务用到的 jQuery API
1. `.on()` -[link](https://www.jquery123.com/on/)
2. `index()` -[link](https://www.jquery123.com/index/)
3. `.eq()` -[link](https://www.jquery123.com/eq/)
4. `.trigger()` -[link](https://www.jquery123.com/trigger/)
5. `.addClass()` -[link](https://www.jquery123.com/addClass/)
6. `.removeClass()` -[link](https://www.jquery123.com/removeClass/)
7. `.siblings()` -[link](https://www.jquery123.com/siblings/)
8. `.css()` -[link](https://www.jquery123.com/css/)

# 优化原则
1. 不使用 CSS 来表示复杂的行为，不要使用IE发明的CSS Expressions。
2. 如果你已经知道图片的宽高了，那么最好是写在上面`<img width=xx height=xx></img>`，这样浏览器就会少一次让后面的图片或者其它元素让位的过程。

# 奇葩bug
1. css tansform: translateX();在浏览器放大页面下出现bug，抖动
