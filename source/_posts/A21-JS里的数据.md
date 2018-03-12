---
title: a21 JS 里的类型
date: 2018-01-15 22:19:20
tags: [JavaScript, 数据类型]
categories: 饥人谷
---

- ## 类型转换
  ### 1. 转换为 **字符串**
    * `toString()` 方法返回一个表示该对象的字符串 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/toString)
    * 语法：`object.toString()`
    * number to string
      ```
      var num = 1;  // number
      n.toString()  // '1'
      ```
    * boolean to string
      ```
      var boolean = true
      boolean.toString()  // 'true'
      ```
    * symbol 这里不研究
    * null
      ```
      var n = null
      n.toString() 
      // Uncaught TypeError: Cannot read property 'toString' of null
      // 所以 null 类型并没有 toString() 这个 api
      ```
    * undefined
      ```
      var u = undefined
      u.toString()
      // Uncaught TypeError: Cannot read property 'toString' of null
      // 所以 undefined 类型也没有 toString() 这个 api
      ```
    * object
      ```
      var obj = {"name": "zero"}
      obj.toString  // "[object Object]"
      // 可以 toString(), 但结果并不是我们想要的
      ```
    * ## 总结：想让一个东西变成字符串，如果它能变成一个字符串，那么直接调用 toString() 就可以了；
      `console.log()`也是用的这种原理，理论上`console.log()`只能接受字符串
      ```
      congole.log('zero')  // zero ,这里浏览器打印出来的并不一定加括号
      console.log(1)  // 1 等于下面的
      console.log((1).toString())
      ```
      object 对象的 `key` 也是这样，
      ```
      var obj = {}
      obj['name'] = 1
      obj[1] = 2  // 1 被 toString()
      obj[true] = 1  // true -> 'true'
      obj  // { 1: 2, name: 1, true: 1 }
      ```
      有些地方自动的、把它需要字符串就调用 `toString()` ,JS 里有很多这种地方
    * 但是使用 toSteing() 很麻烦，下面是老司机的方法，与**空字符串相加**
      使用 `+` 连接符，如果它的一边有字符串，它就会尝试将另外一边也变成字符串
      ```
      1 + ''  // '1'
      true + '' // 'true'
      var obj = {}
      obj + '' // '[Object Object]'
      null + ''  // 'null'
      undefined + '' // 'undefined'
      也可以翻过写
      '' + 1  // '1'
      '' + true  // 'true'
      ...
      这种方式甚至比 toString() 更强大，因为它能将 null 和 undefined 也变成字符串
      ```
    * #### 使用`String()`函数，可以将任意类型的值转化成字符串。转换规则如下。
      1. 原始类型值的转换规则
        > **数值：**转为相应的字符串。
        > **字符串：**转换后还是原来的值。
        > **布尔值：**true转为"true"，false转为"false"。
        > **undefined：**转为"undefined"。
        > **null：**转为"null"。
        
        ```
        String(123) // "123"
        String('abc') // "abc"
        String(true) // "true"
        String(undefined) // "undefined"
        String(null) // "null"
        ```
      2. 对象的转换规则
        String方法的参数如果是对象，返回一个类型字符串；如果是数组，返回该数组的字符串形式。
        `String({a: 1}) // "[object Object]"`
        `String([1, 2, 3]) // "1,2,3"`
      3. [阮一峰](http://javascript.ruanyifeng.com/grammar/conversion.html#toc2)
      [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
  ### 2. 转换为 **布尔值**
    * `Boolean()`函数，可以将任意类型的变量转为布尔值。(强制转换)
    * 它的转换规则相对简单：除了以下六个值的转换结果为false，其他的值全部为true。
      > undefined
      > null
      > -0
      > 0或+0
      > NaN
      > ''（空字符串）
      ```
      Boolean(undefined) // false
      Boolean(null) // false
      Boolean(0) // false
      Boolean(NaN) // false
      Boolean('') // false
      Boolean(1)  // true
      Boolean(' ')  // true
      ```
    * 注意，所有对象（包括空对象）的转换结果都是true，甚至连false对应的布尔对象new Boolean(false)也是true。
      ```
      Boolean({}) // true
      Boolean([]) // true
      Boolean(new Boolean(false)) // true
      ```
    * 所有对象的布尔值都是true，这是因为JavaScript语言设计的时候，出于性能的考虑，如果对象需要计算才能得到布尔值，对于obj1 && obj2这样的场景，可能会需要较多的计算。为了保证性能，就统一规定，对象的布尔值为true。
    * [阮一峰](http://javascript.ruanyifeng.com/grammar/conversion.html#toc3)
    * [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
    * **老司机的简易方法**
    * 首先来看一段代码
      `!trur  // false`
      **(`!`是取反的意思)**
      那么在前面再加一个`!`
      `!!true  // true`
      得到这样一个结果（可以尝试理解负负得正）
      于是我们得到一个结果，用任何一个东西给它取反两次就会得到它的布尔值
      ```
      !1  // false
      !!true  // true
      !!0  // false
      !!1  // true
      !!''  // false
      !!' '  // true
      !!null  // false
      !!undefined  // false
      !!{}  // true
      !!{name: 'zero'}  // true
      ```
      它的规则和`Boolean()`是一样的，只有那几个值是'false'
    * [falsy-MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)
  ### 3. 转换为**Number**
    * '1' -> 1
      ```
      1. Number('1') === 1
      2. parseInt('1', 10) === 1
      3. parseFloat('1.23') === 1.23
      // 老司机的简单方法
      4. '1' - 0 === 1
      5. + '1' === 1
      6. -(- '1') === 1

      // 还有一些需要注意的
      parseInt('011') === 11
      parseInt('011', 8) === 9
      parseInt('011', 10) === 11
      parseInt('12s') === 12
      ```
    * [阮一峰](http://javascript.ruanyifeng.com/grammar/conversion.html#toc1)
- ## 内存图
  + ![](http://upload-images.jianshu.io/upload_images/9047034-91883100db3b875c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  + ![](http://upload-images.jianshu.io/upload_images/9047034-e1878e7a431c7eba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  + ![](http://upload-images.jianshu.io/upload_images/9047034-7af74d1ed5c38193.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  + 面试题
    ```
    var a = 1
    var b = a
    b = 2
    请问 a 显示是几？  
    1
  
    var a = {name: 'a'}
    var b = a
    b = {name: 'b'}
    请问现在 a.name 是多少？
    'a'
  
    var a = {name: 'a'}
    var b = a
    b.name = 'b'
    请问现在 a.name 是多少？
    'b'
  
    var a = {name: 'a'}
    var b = a
    b = null
    请问现在 a 是什么？
    {name: 'a'}

    var a = {n: 1}
    var b = a;
    a.x = a = {n: 2}  // 先计算 a = {n: 2},再计算 a.x = 值
    //  难点在于，a = {n: 2}计算后，a储存的地址已经变了，
    //  但是a.x的a的地址仍然是之前的地址,详见下图

    alert(a.x)  // undefined
    alert(b.x)  // [object Object]
    // alert函数默认参数使用toString转换字符串
    ```
    ![](http://upload-images.jianshu.io/upload_images/9047034-53ad79e8a6b03a6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  + 垃圾回收
    如果一个对象没有被引用，她就是垃圾，**将**被回收
    ```
    var a = {name: 'a'}
    var b = {name: 'b'}
    a = b
    // {name: 'a'} 失去引用，将被回收
    ```
    ![](http://upload-images.jianshu.io/upload_images/9047034-008f46a1ca34ac7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  + 深复制
    ```
    var a = 1
    var b = a
    b = 2 //这个时候改变 b
    a 完全不受 b 的影响
    那么我们就说这是一个深复制
    ```
    对于简单类型的数据来说，赋值就是深拷贝。

    对于复杂类型的数据（对象）来说，才要区分浅拷贝和深拷贝。
    ```
    // 这是一个浅拷贝的例子
    var a = {name: 'frank'}
    var b = a
    b.name = 'b'
    a.name === 'b' // true
    //  因为我们对 b 操作后，a 也变了
    ```
    什么是深拷贝了，就是对 Heap 内存进行完全的拷贝。
    ```
    var a = {name: 'frank'}
    var b = deepClone(a) // deepClone 还不知道怎么实现
    b.name = 'b'
    a.name === 'a' // true
    ```
    ![](http://upload-images.jianshu.io/upload_images/9047034-8e1d24d495480a47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
