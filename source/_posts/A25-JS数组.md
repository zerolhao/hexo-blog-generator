---
title: A25-JS数组
date: 2018-01-20 23:34:15
tags:
---

- ## `Array` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
  window.Array 全局对象（也是函数）
  - 基本用法:
    ```
    var a = Array(3)  // {length: 3}
    a.length // 3
    0 in a // false, 数组 a 里面并没有 0 这个 key
    a[0] // undefined
    a.push('three')
    3 in a // true
    a[3] // 'three'
    ```
    ![](http://upload-images.jianshu.io/upload_images/9047034-b91253072572d58b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    ```
    var a2 = Array(3,3) // [3,3]
    a2.length // 2
    0 in a2 // true
    a2[0] // 3 
    // 当括号里只有一个数字参数的时候，它是长度，当参数不止一个的时候它是参数
    // 这叫做不一致性，这不是一件好事
    ```
    ![](http://upload-images.jianshu.io/upload_images/9047034-c979044ed221b927.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  
    ```
    new Array(3) 跟不加 new 一样的效果
    new Array(3,3,) 跟不加 new 一样的效果
  
    var a = [1,2,3] // 常用简写方式
    ```
  - 加不加`new`对不同类型的区别：
    - `String、Number、Boolean`
      `String()`返回基本类型
      `new String()`返回对象
    - `Object、Array、Function`
      `Object()`返回对象
      `new Object()`返回依然是对象
  - 数组为什么是数组而不是对象是因为数组有着对象没有的特点
    也就是你的对象的`__proto__`指向的是否是`Array.prototype`
    ![](http://upload-images.jianshu.io/upload_images/9047034-3f3ffb2c07e02a89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - 数组的一个特征
    数组之所以是数组，其实是因为你把它数组
    ```
    var a = [1,2,3]
    a.x = 'x'
    a.y = 'y'
    for(let i=0; i < a.length; i++){ // 这种方法遍历排除了 x、y
      console.log(a[i]) // 1, 2, 3
    }
    for(let key in a){
      console.log(key) // 0, 1, 2, x, y
    }
    var obj = {
      0: 1, 1: 2, 2: 3, length: 3
    }
    for(let i=0; i < obj.length; i++){
      console.log(obj[i]) // 1, 2, 3
    }
    ```
  - 伪数组
    上面的`obj`对象就是伪数组；它不关心你有没有数组的那些方法，只需要012345……这些下标去遍历
    只要你的原型链中没有`Array.prototype`就是伪数组
    目前JS中只有一种伪数组，就是`arguments`
    ```
    function f(){ console.dir(arguments) }
    f(1,2,3)
    ```   
    从下图可以看见它引用的是`Object.protype`而非`Array.prototype`
    ![](http://upload-images.jianshu.io/upload_images/9047034-54e385e6ddc2d1bb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  - `forEach`API -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
    forEach() 方法对数组的每个元素执行一次提供的函数。




- ## `Function` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)
  ```
  // 常用简写方式
  var f = function(a,b){
    return a+b
  }

  var f = new Function('a', 'b', 'return a+b') //使用 new 进行构造，
  var f = new Function(['a','b'],'return a+b')
  ```
  语法：`new Function ([arg1[, arg2[, ...argN]],] functionBody)`
  参数：
  `arg1, arg2, ... argN`
  被函数使用的参数的名称必须是合法命名的。参数名称是一个有效的JavaScript标识符的字符串，或者一个用逗号分隔的有效字符串的列表;例如“×”，“theValue”，或“A，B”。
  `functionBody`
  一个含有包括函数定义的JavaScript语句的字符串
  
  函数总是会返回一个值
  ```
  function fn(){
    console.log('函数')
    return undefined // 如果你不写就会自动添加这样的一句
  }
  ```