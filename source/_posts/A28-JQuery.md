---
title: A28-JQuery
date: 2018-01-26 23:43:33
tags: [jQuery]
categories: 饥人谷
---

# 这次我们自己实现一个类似Query的API（简化版）

## 封装函数
### 获取一个元素所有兄弟元素
```
// html
<ul>
  <li id="item1">选项1</li>
  <li id="item2">选项2</li>
  <li id="item3">选项3</li>
  <li id="item4">选项4</li>
  <li id="item5">选项5</li>
</ul>

// js, 假设我们获取 item3 的所有兄弟元素
var allChildren = item3.parentNode.children
var array = {
  length: 0;
}
for(let i=0; i < allChildren.length; i++){
  if(allChildren[i] !== item3){
    array[array.length] = allChildren[i]
    array.length += 1
  }
}
console.log(array)  // 测试
// 这样就获得 item3的所有兄弟元素了
// 接下来我们将这段代码封装起来
function getSiblings(node){ /* API */
  var allChildren = node.parentNode.children
  var array = {
    length: 0;
  }
  for(let i=0; i < allChildren.length; i++){
    if(allChildren[i] !== node){
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array  // 返回一个伪数组
}
console.log(getSiblings(item2)) // test
// 这样，一个api就做好了，它可以获得一个元素的所有兄弟元素
```
### 接下来我们开始给一个元素添加  class
仍然是上面那段 `html`
```
// 为元素添加多个 class
item3.classList.add('a')
item3.classList.add('b')
item3.classList.add('c')

// 简洁一点的写法
var classes = ['a','b','c']
classes.forEach( value => item3.classList.add(value) )

// 这次我们既可以 add 也可以 remove
var classes = {'a':true, 'b':false, 'c':true}
for(let key in classes){
  if(classes[key]){
    item3.classList.add(key)
  }
  else{
    item3.classList.remove(key)
  }
}

// 封装成函数
function addClass(node, classes){
  for(let key in classes){
    /*
    if(classes[key]){
      node.classList.add(key)
    }
    else{
      node.classList.remove(key)
    }
    */
    // 代码优化守则1：如果出现类似的代码，就存在优化的可能
    // 优化这段代码
    let methodName = class[key] ? 'add' : 'remove'
    node.classList[methodName](key)
  }
}
addClass(item3, {a:true,b:false,c:true}) // test
```

## 命名空间
上面两个函数是有关联性的，他们都是对node进行操作
现在我们来声明一个变量，将它们放进去
```
window.zdom = {}

zdom.getSiblings = function(node){
  var allChildren = node.parentNode.children
  var array = {
    length: 0;
  }
  for(let i=0; i < allChildren.length; i++){
    if(allChildren[i] !== node){
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array  // 返回一个伪数组
}

zdom.addClass = function(node, classes){
  for(let key in classes){
    let methodName = class[key] ? 'add' : 'remove'
    node.classList[methodName](key)
  }
}

zdom.getSiblings(item3)
zdom.addClass(item3,{a: true, b: true, c: flase})
```
有一个库就是这样的方式：yui
这就叫做命名空间，也是一种设计模式。所有的套路都是设计模式，就比如哈希，就比如数组。
这种常用的模式或者组合就叫做设计模式。
为什么要命名空间：想想JQuery，那么多api，难道对别人说的时候要一个一个报出来么，所以一个统称的名字就很必要了，同时，你怎么知道别人没写addClass呢？全部放到window里不会产生覆盖么，所以，
如果没有命名空间：
1. 别人不知道你的库叫什么
2. 你会不知不觉把所有全局变量都覆盖了

## 将 node 放在前面
相较于命名空间的调用方式(这种方法已经过时了)
`zdom.getSiblings(item3)`
`zdom.addClass(item3,{a: true, b: true, c: flase})`
我们认为这样的方式更好：
`item3.getSiblings()`
`item3.addClass({a: true, b: true, c: flase})`

这时候，就需要用到原型，来扩展 Node 接口，
**这是第一种方法**
```
Node.prototype.getSiblings = function(){
  var allChildren = this.parentNode.children
  var array = {
    length: 0;
  }
  for(let i=0; i < allChildren.length; i++){
    if(allChildren[i] !== this){
      array[array.length] = allChildren[i]
      array.length += 1
    }
  }
  return array  // 返回一个伪数组
}
Node.prototype.addClass = function(classes){
  for(let key in classes){
    let methodName = class[key] ? 'add' : 'remove'
    this.classList[methodName](key)
  }
}
// 隐式指定 this
item3.getSiblings()
item3.addClass({a: true, b: true, c: flase})
// 显式指定 this
item3.getSiblings.call(item3)
item3.addClass.call(item3, {a: true, b: true, c: flase})
```
但是改写Node.prototype可能会出现覆盖情况，所以又有了**第二种方法**：
这种叫做「无侵入」
```
window.Node2 = function(node){
  return {
    getSiblings: function(){
      var allChildren = node.parentNode.children
      var array = {
        length: 0;
      }
      for(let i=0; i < allChildren.length; i++){
        if(allChildren[i] !== node){
          array[array.length] = allChildren[i]
          array.length += 1
        }
      }
      return array  // 返回一个伪数组
    },
    addClass: function(classes){
      for(let key in classes){
        let methodName = class[key] ? 'add' : 'remove'
        node.classList[methodName](key)
      }
    }
  }
}
var node2 = Node2(item3)
node2.getSiblings()
node2.addClass({a: true, b: true, c: flase})
```
### 给 Node2 换个名字 jQuery
```
window.jQuery = function(node){
  return {
    getSiblings: function(){ ... },
    addClass: function(classes){ ... }
  }
}
var node2 = jQuery(item3)
node2.getSiblings()
node2.addClass({a: true, b: true, c: flase})
```
除了名字换了一下，和刚才的代码并没有区别
只是Node2变成了jQuery
**jQuery就是一个这么升级了的DOM**
**它接受一个旧的节点，然后返回你一个新的对象**
这个新的对象有新的API，它的内部实现依然是去调用旧的API，只是变得更好用、更方便（一句话相当于以前十句话）
然而jQuery的厉害不止于此，它还可以接受选择器
```
window.jQuery = function(nodeOrSelector){
  let node
  // 类型检测
  if(typeof nodeOrSelector === 'string'){
    node = document.querySelector(nodeOrSelector)
  } else{
    node = nodeOrSelector
  }
  return {
    getSiblings: function(){
      var allChildren = node.parentNode.children
      var array = {  length: 0; }
      for(let i=0; i < allChildren.length; i++){
        if(allChildren[i] !== node){ // 注意这里用到了闭包，匿名函数访问了外面的 node
          array[array.length] = allChildren[i]  // node 和 匿名函数组成了闭包（下面的函数同理）
          array.length += 1
        }
      }
      return array  // 返回一个伪数组
    },
    addClass: function(classes){
      for(let key in classes){
        let methodName = class[key] ? 'add' : 'remove'
        node.classList[methodName](key)
      }
    }
  }
}
var node2 = jQuery('#item3')
// var node2 = jQuery('ul > li:nth-child(3)')
node2.addClass({a: true, b: true, c: flase})
```
#### 那么如果要操作多个节点呢？
```
window.jQuery = function(nodeOrSelector){
  let nodes = {}
  if(typeof nodeOrSelector === 'string'){
    let temp = document.querySelectorAll(nodeOrSelector) // 伪数组
    for(let i=0; i < temp.length; i++){ // 如果你想要一个纯净的伪数组不要那些其它属性方法
      nodes[i] = temp[i]
    }
    node.length = temp.length
  } else if(nodeOrSelector instanceof Node) {
    nodes = {0: nodeOrSelector, length: 1}
  }
  nodes.getSiblings = function(){/*太麻烦不做了*/}
  nodes.addClass = function(classes){
    classes.forEach((value) => {
      for(let i=0; i < nodes.length; i++){
        nodes[i].classList.add(value)
      }
    })
  }
  // 添加几个有用的 jQuery Api
  nodes.getText = function(){
    let texts = []
    for(let i=0; i < nodes.length; i++){
      texts.push(nodes[i].textContent)
    }
    return texts
  }
  nodes.setText = function(text){
    for(let i=0; i < nodes.length; i++){
      nodes[i].textContent = text
    }
  }
  // 然而实际上 jQuery 合并了 get和set
  // 类似的 api jquery 有很多
  nodes.text = function(text){
    // jQuery 认为，你没有设置参数，那就是要获取 text，
    // 如果有参数，那就是要设置 text
    if(text === undefined){
      let texts = []
      for(let i=0; i < nodes.length; i++){
        texts.push(nodes[i].textContent)
      }
      return texts
    } else{
      for(let i=0; i < nodes.length; i++){
        nodes[i].textContent = text
      }
    }
  }
  return nodes
}

var node2 = jQuery('ul > li')
node2.addClass(['red'])
var text = node2.text()
node2.text('hi')
```
#### 缩写 alias
```
window.$ = jQuery
// 建议使用 jQuery 构造出来的 对象都在前面加一个 $ ,以表示它是由$构造的，避免和正常的弄混
var $node2 = $('ul>li')
```
# 知道了 jQuery 是怎么实现的，现在去看看真正的 jQuery吧 -[MDN](https://jquery.com/)
- [jQuery Api](http://api.jquery.com/)
- [jQuery Api中文网](https://www.jquery123.com/)
```
// html
<ul>
  <li class='red'>选项1</li>
  <li class='red'>选项2</li>
  <li class='red'>选项3</li>
  <li class='red'>选项4</li>
  <li class='red'>选项5</li>
</ul>
<button id=x></button>

// css
.red{color:red;}
.green{color:green;}

// js,使用 jQuery
var nodes = jQuery('ul>li')
var classes = ['red','green','blue','yellow','black']
x.onclick = function(){
  /* 常规写法
  nodes.removeClass('red')
  nodes.addClass('green')
  */
  nodes.removeClass('red').addClass('green') // 链式操作
}
```

# 总结
现在大概已经知道 jQuery 是怎么写的了，明白了它的原理，接下来只需要多了解它的api，
但实际上 jQuery 并没有这么简单，作为曾经风靡前端的库，它的强大超乎我们的想象。
- jQuery 在兼容性方面做得很好，1.7 版本兼容到 IE 6
- jQuery 还有动画、AJAX 等模块，不止 DOM 操作
- jQuery 的功能更丰富
- jQuery 使用了 prototype，我们没有使用，等学了 new 之后再用 

**库是特定种类的API**
