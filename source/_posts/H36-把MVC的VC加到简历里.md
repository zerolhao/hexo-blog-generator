---
title: H36-把MVC的VC加到简历里
date: 2018-03-09 00:04:37
tags:
---
# 模块化
简单来说就是将功能不同的代码分开来，实现a功能的代码一个文件，实现b功能的代码一个文件
[阮一峰](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)
> 模块就是实现特定功能的一组方法。
> 只要把不同的函数（以及记录状态的变量）简单地放在一起，就算是一个模块。

[ES6 Module](http://es6.ruanyifeng.com/#docs/module)
[Web Components](http://javascript.ruanyifeng.com/htmlapi/webcomponents.html)
[知乎问答](https://www.zhihu.com/question/37649318)

# 如何使用立即执行函数
1. 我们不想要全局变量
2. 我们要使用局部变量
3. ES 5 里面，只有函数有局部变量
4. 于是我们声明一个 function xxx，然后 xxx.call()
5. 这个时候 xxx 是全局变量（全局函数）
6. 所以我们不能给这个函数名字
7. function(){}.call()
8. 但是 Chrome 报错，语法错误
9. 试出来一种方法可以不报错:
  1. !function(){}.call() (我们不在乎这个匿名函数的返回值，所以加个 ! 取反没关系)
  2. (function(){}).call() 方方不推荐
      xxx    
      (function(){}).call() 报错
  3. frank192837192463981273912873098127912378.call() 不推荐

# 如何使用闭包
1. 立即执行函数使得 person 无法被外部访问
2. 闭包使得匿名函数可以操作 person
3. window.frankGrowUp 保存了匿名函数的地址
4. 任何地方都可以使用 window.frankGrowUp
  => 任何地方都可以使用 window.frankGrowUp 操作 person，但是不能直接访问 person

# 箭头函数没有 this
箭头函数内外 this 不变，
```
!function() {
  var view = document.querySelector('#siteTopNavBar')
  var controller = {
    view: null,
    init: function(view) {
      this.view = view
      this.bindEvents()
    },
    bindEvents: function() {
      this.userDoScroll()
      window.addEventListener('scroll',()=>{
        this.userDoScroll()
      })// 箭头函数没有this，自动引用外层的this
      
      // 注意this指向，下面这样写this就会变成指向window
      //window.addEventListener('scroll', this.userDoScroll) // 等于下面
      //window.onscroll = function(){...}
      
      //或者使用 bind 绑定this 
      // window.addEventListener('scroll',this.userDoScroll.bind(this))
    },
    userDoScroll: function(){
      if (window.scrollY !== 0) {
        this.active()
      } else {
        this.deactive()
      }
      //console.log(this)
    },
    active: function(){
      this.view.classList.add('sticky')
    },
    deactive: function(){
      this.view.classList.remove('sticky')
    }
  }

  controller.init(view)
}.call()
```

# MVC
[wiki](https://www.wikiwand.com/zh-hans/MVC)
> MVC模式（Model–view–controller）是软件工程中的一种软件架构模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。
> MVC模式的目的是实现一种动态的程式设计，使后续对程序的修改和扩展简化，并且使程序某一部分的重复利用成为可能。
> 除此之外，此模式通过对复杂度的简化，使程序结构更加直观。软件系统通过对自身基本部分分离的同时也赋予了各个基本部分应有的功能。

- 控制器（Controller）- 负责转发请求，对请求进行处理。
- 视图（View） - 界面设计人员进行图形界面设计。
- 模型（Model） - 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。
![](http://upload-images.jianshu.io/upload_images/9047034-617d66a32b4054aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/9047034-bc898ca087615df7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[MDN MVC architecture](https://developer.mozilla.org/en-US/Apps/Fundamentals/Modern_web_app_architecture/MVC_architecture)
[阮一峰-谈谈MVC模式](http://www.ruanyifeng.com/blog/2007/11/mvc.html)
[阮一峰-MVC，MVP 和 MVVM 的图示](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)
[方应杭-后端的 MVC](https://zhuanlan.zhihu.com/p/22834622)
[方应杭-前端的 MVC](https://zhuanlan.zhihu.com/p/22943208)