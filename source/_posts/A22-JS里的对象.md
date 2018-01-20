---
title: A22-JS里的对象
date: 2018-01-17 23:37:46
tags:
---

- ## 全局对象 window
  ECMAScript 规定全局对象叫做 global，但是浏览器把 window 作为全局对象（浏览器先存在的）
  window 就是一个哈希表，有很多属性。
  window 的属性就是全局变量。
  使用window的属性可以不加`window.`,
  比如`window.alert('警告')`可以写成`alert('警告')`
  这些全局变量分为两种：

  一种是 ECMAScript 规定的
  + global.parseInt
  + global.parseFloat
  + global.Number
  + global.String
  + global.Boolean
  + global.Object

  一种是浏览器自己加的属性
  + window.alert
  + window.prompt
  + window.comfirm
  + window.console.log
  + window.console.dir
  + window.document
    // document由浏览器实现，但是也有一个自己的标准，那就是 DOM，它的标准由 W3C 制定
  + window.document.createElement
  + window.document.getElementById
  所有 API 都可以在 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 里找到详细的资料。

- ## 全局函数&简单类型与对象的区别
  ### 1. Number()
    + [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number) 
    + [阮一峰](http://javascript.ruanyifeng.com/stdlib/number.html#toc6)
    + `Number('1')  // 1 强制转换`
    + `var n = new Number(1)  // 返回一个值为1的对象`
      在这里，Number()对象作为构造函数使用，用来生成值为数值的对象。
    + 和直接赋值数字也就是简单类型的区别：
      * 一个是简单类型，一个是复杂类型
      * 两者对内存的使用不同，假设有
        `var n1 = 1`
        `var n2 = new Number(1)`
        内存图：![](http://upload-images.jianshu.io/upload_images/9047034-c00bbbd2359c024d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      * 包装对象有许多便捷的操作
      * 你可以通过`valueOf()`来获得`n2`的原始值`1`
      * 其它还有
        ```
        toString() // 获得字符串形式
        toFixed() // 转为指定位数的小数，返回这个小数对应的字符串
        toExponential() // 将一个数转为科学计数法形式
        toPrecision() // 将一个数转为指定位数的有效数字

        (10).toString() // "10"
        // toString方法可以接受一个参数，表示输出的进制。如果省略这个参数，默认将数值先转为十进制，再输出字符串；否则，就根据参数指定的进制，将一个数字转化成某个进制的字符串。
        (10).toString(2) // "1010"
        (10).toString(8) // "12"
        (10).toString(16) // "a"

        // 上面代码中，之所以要把10放在括号里，是为了表明10是一个单独的数值，后面的点表示调用对象属性。如果不加括号，这个点会被JavaScript引擎解释成小数点，从而报错。

        // 只要能够让JavaScript引擎不混淆小数点和对象的点运算符，各种写法都能用。除了为10加上括号，还可以在10后面加两个点，JavaScript会把第一个点理解成小数点（即10.0），把第二个点理解成调用对象属性，从而得到正确结果。
        10..toString(2) // "1010"
        
        // 其他方法还包括
        10 .toString(2) // "1010"
        10.0.toString(2) // "1010"

        // toFixed方法用于将一个数转为指定位数的小数，返回这个小数对应的字符串。
        (10).toFixed(2) // "10.00"
        10.005.toFixed(2) // "10.01"

        // toExponential方法的参数表示小数点后有效数字的位数，范围为0到20，超出这个范围，会抛出一个RangeError。
        (10).toExponential()  // "1e+1"
        (10).toExponential(1) // "1.0e+1"
        (10).toExponential(2) // "1.00e+1"
        
        (1234).toExponential()  // "1.234e+3"
        (1234).toExponential(1) // "1.2e+3"
        (1234).toExponential(2) // "1.23e+3"

        // toPrecision方法的参数为有效数字的位数，范围是1到21，超出这个范围会抛出RangeError错误。
        (12.34).toPrecision(1) // "1e+1"
        (12.34).toPrecision(2) // "12"
        (12.34).toPrecision(3) // "12.3"
        (12.34).toPrecision(4) // "12.34"
        (12.34).toPrecision(5) // "12.340"
        ```
      * 两种赋值方式的历史：
        `var n = new Number(1)`是当初被要求要像java才做出来的方式，实际上更受人喜欢的依然是`var n = 1`，
        但是这样方式无法使用Number对象的方法怎么办，于是又有了下面的东西；
      * 基本包装类型
        理论上基本类型是不能没有`toString()`之类的方法的，然而实际上却可以使用，
        这是因为每当地区一个基本类型值的时候，后台就会创建一个对应的基本包装类型的对象，从而让我们能够调用一些方法来操作这些数据，如下：
        ```
        var n = 1  // 1
        var s = n.toString()  // '1'
        // 后台自动操作
        var temp = new Number(n)  // 创建一个 Number 类型的实例
        temp.toString()  // 调用指定方法
        // 然后将temp.toString()的值作为n.toString()的值
        // 最后销毁temp，释放空间
        // 所以这是一种临时的转换，基本类型实际上并没有属性，
        ```
        也因此，`var n = new Number(1)`这种方式基本不再被大家所使用
      * 思考题
        ```
        var n = 1
        n.xxx = 2
        // 问：是否会报错？
        // NO!
        n.xxx  = 2 // 2
        n.xxx  // undefined
        ```
        解析：n.xxx = 2这段代码执行时新建了一个临时变量temp以及属性xxx储存值2，所以不会报错，
        但是执行 n.xxx 的时候浏览器显示的却是 undefined，这是因为 temp 是临时创建出来了，使用后会立即销毁，执行 n.xxx 的时候上一句代码创建的临时temp已经被销毁了，这时候又是一个新的temp，它并没有xxx这个属性以及值
        ![](http://upload-images.jianshu.io/upload_images/9047034-e4c77c7406ce4aa3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      * String()和Boolean()也是同理
      * [阮一峰](http://javascript.ruanyifeng.com/stdlib/wrapper.html)
      * JavaScript高级程序设计第五章-5.6-基本包装类型
  ### 2. String()
    + [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
    + [MDN学习教程](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/Useful_string_methods)
    + [阮一峰](http://javascript.ruanyifeng.com/stdlib/string.html)
    + ```
      var s = 'abc'
      var s2 = new String(s)

      // charAt方法返回指定位置的字符，参数是从0开始编号的位置。
      s.charAt(1) // "b"
      s.charAt(s.length - 1) // "c"
      // 这个方法完全可以用数组下标替代。
      'abc'.charAt(1) // "b"
      'abc'[1] // "b"
      // 如果参数为负数，或大于等于字符串的长度，charAt返回空字符串。
      'abc'.charAt(-1) // ""
      'abc'.charAt(3) // ""

      // charCodeAt方法返回给定位置字符的Unicode码点（十进制表示），相当于String.fromCharCode()的逆操作。
      'abc'.charCodeAt(1) // 98
      // 如果没有任何参数，charCodeAt返回首字符的Unicode码点。
      'abc'.charCodeAt() // 97
      // 如何返回 16 进制表示的Unicode码点
      'abc'.charCodeAt(0).toString(16)

      // trim方法用于去除字符串两端的空格，返回一个新字符串，不改变原字符串。
      '  hello world  '.trim()  // "hello world"
      // 该方法去除的不仅是空格，还包括制表符（\t、\v）、换行符（\n）和回车符（\r）。
      '\r\nabc \t'.trim()  // 'abc'

      // concat方法用于连接两个字符串，返回一个新字符串，不改变原字符串。
      var s1 = 'abc';
      var s2 = 'def';
      s1.concat(s2) // "abcdef"
      s1  // "abc"
      s2  // "def"
      // 该方法可以接受多个参数。
      'a'.concat('b', 'c') // "abc"

      // slice方法用于从原字符串取出子字符串并返回，不改变原字符串。
      // 它的第一个参数是子字符串的开始位置，第二个参数是子字符串的结束位置（不含该位置）。
      'JavaScript'.slice(0, 4) // "Java"
      // 如果省略第二个参数，则表示子字符串一直到原字符串结束。
      'JavaScript'.slice(4) // "Script"
      // 如果参数是负值，表示从结尾开始倒数计算的位置，即该负值加上字符串长度。
      'JavaScript'.slice(-6) // "Script"
      'JavaScript'.slice(0, -6) // "Java"
      'JavaScript'.slice(-2, -1) // "p"
      // 如果第一个参数大于第二个参数，slice方法返回一个空字符串。
      'JavaScript'.slice(2, 1) // ""

      ```
    + String()有很多api，这里只写了几个常用的，更多详见上面两个链接
  ### 3. Boolean()
    + [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
    + [阮一峰](http://javascript.ruanyifeng.com/stdlib/wrapper.html#toc6)
    + ```
      var b = new Boolean(true)
      b.toString()  // 'true'
      b.valueOf()  // true

      // 踩坑
      var f = false
      var f2 = new Boolean(false)
      if(f) { console.log(1) }
      if(f2) { console.log(2) }
      // 问：会打印出什么？
      2  // 只打印 2，因为 f2 并不是 false，因为它是一个对象，而对象是 true     
      // 别忘记js中只有 NAN、null、undefined、''、0是false，而对象和其它的都是true
      ```
  ### 4. Object()
    + [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)
    + [阮一峰](http://javascript.ruanyifeng.com/stdlib/object.html)
    + ```
      var o = {}
      var o2 = new Object()
      // 这两种写法的结果没有任何区别，因为 对象 本身是复杂类型，但是
      o === o2  // false
      // 这是因为它们虽然都创建一个空对象，但是地址并不一样，是两个地址不同拥有自己空间的空对象
      ```
- ## 公共属性（原型）
  ```
  var n = new Numer(1)
  var s = new String('a')
  var b = new Boolean(true)
  var o = new Object()

  // 上面4个不同的对象都有`toString()、valueOf()`属性，可是如果每个对象都创建2个属性会很浪费内存，
  // 所以，共用就好了
  ```
  ![](http://upload-images.jianshu.io/upload_images/9047034-0f6ff979ff671a46.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  那么怎么访问公共属性呢？每个对象的toString储存公共属性的toString的地址？不
  + __proto__
    js通过隐藏属性`__proto__`属性来访问公共属性，它指向了那些所有对象共有的属性
    ![](http://upload-images.jianshu.io/upload_images/9047034-c5f22ebf1be96355.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    ![](http://upload-images.jianshu.io/upload_images/9047034-58a951ddf11b50f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    步骤：
    1. 首先查看自身是否是对象，如果不是就包装对象
    2. 如果是对象就查看自身的 key 有没有 toString，有的话就直接调用
    3. 如果没有的话就通过`__proto__`查看你的共用属性看有没有，有的话就将你自身对应的值打印出来
    例子：
    ```
    var o1 = {name: 'zero'}
    o1.toString()  // [object Object]
    // 依照步骤来，o1是对象，o1没有toString，查看共有属性，有toString,打印o1对应的值
    // 不仅仅是o1,创建更多的对象也是一样，所有的对象都有隐藏属性`__proto__`，指向对应的共有属性
    var o2 = {}
    o1 === o2  // false
    o1.toString() === o2.toString()  // true
    ```
  + 如果是Number对象呢？
    ```
    var n = new Number(11)
    n.toString(16) // numer对象可以这样，而oject对象是不能这样toString(16)的
    ```
    ![](http://upload-images.jianshu.io/upload_images/9047034-73d1a598cc39ecd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    ![](http://upload-images.jianshu.io/upload_images/9047034-7053ffb0f13ffd7e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
    可以看见，Number重写了toString属性，以及添加一部分自己的属性，
    然后通过`__proto__`指向Object，对象共有属性
    所以如同上图，Number对象就是在第3步去Number重写的属性寻找，没有的话去共有属性找
    Number对象的`__proto__`指向Number重写的共有属性，Number重写的共有属性的`__proto__`指向对象共有属性
  + 除了Number以外，类似的还有String、Boolean(目前所学的)
    ![](http://upload-images.jianshu.io/upload_images/9047034-735c75fa120b9192.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  + 原型链
    * 对象的属性和方法，有可能定义在自身，也有可能定义在它的原型对象。由于原型本身也是对象，又有自己的原型，所以形成了一条原型链（prototype chain）,就像我们上面画的图一样，
    * 如果一层层地上溯，所有对象的原型最终都可以上溯到Object.prototype，即Object构造函数的prototype属性。Object.prototype 就是 Object对象的共有属性，相对的，Number.prototype就是Number对象的共有属性，而Number.prototype的__proto__又指向Object.prototype,也就是它的共有属性，String和Boolean也是如此
      ![](http://upload-images.jianshu.io/upload_images/9047034-078fe00b7805d361.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      ![](http://upload-images.jianshu.io/upload_images/9047034-0d483aad5063c4a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      ![](http://upload-images.jianshu.io/upload_images/9047034-025562d3dd0f9ab8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      看这张图，结合前面所学的数据结构，是不是很有意思，
      根节点是Object.prototype,三个子节点分别是String.prototype、Number.prototype、Boolean.prototype,
      当你执行`var str = new String('a')`时，浏览器就会在堆中创建一个hash，str保存这个hash的地址，
      这个hash也就是String对象中除了你定义的属性外还有隐藏属性__proto__指向String.prototype,String.prototype又指向Object.prototype,这就完成了String API的所有绑定，
      Number和Boolean也是一样
    * “原型链”的作用是，读取对象的某个属性时，JavaScript 引擎先寻找对象本身的属性，如果找不到，就到它的原型去找，如果还是找不到，就到原型的原型去找。如果直到最顶层的Object.prototype还是找不到，则返回undefined。
    * 如果对象自身和它的原型，都定义了一个同名属性，那么优先读取对象自身的属性，这叫做“覆盖”（overriding）。
    * 需要注意的是，一级级向上，在原型链寻找某个属性，对性能是有影响的。所寻找的属性在越上层的原型对象，对性能的影响越大。如果寻找某个不存在的属性，将会遍历整个原型链。
    * prototype与__proto__
      ![](http://upload-images.jianshu.io/upload_images/9047034-96c0b1e26bda2589.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      两者都指向共用属性
      String.prototype 是 String 的共用属性的引用（防止共用属性被垃圾回收）
      s.__proto__ 是 String 的共用属性的引用（这是你在用共用属性）

      ![](http://upload-images.jianshu.io/upload_images/9047034-824231e631c041b0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      ![](http://upload-images.jianshu.io/upload_images/9047034-18e698f7dbc6f91d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      __proto__ 是对象的属性，prototype 是函数的属性
      它们储存的地址相同，指向的其实是同一个对象
      ```
      // 一些烧脑的东西

      var obj1 = 函数.prototype
      obj1.__proto__  === Object.prototype  // 等于下面
      函数.prototype.__proto__ === Object.prototype

      var obj2 = 函数
      obj2.__proto__ === Function.prototype  // 也就是
      函数.__proto__ === Function.prototype
      Function.__proto__ === Function.prototype
      Function.prototype.__proto__ === Obeject.prototype

      String.__proto__ === Function.prototype
      Number.__proto__ === Function.prototype
      Boolean.__proto__ === Function.prototype
      Object.__proto__ === Function.prototype
      ```
      ![](http://upload-images.jianshu.io/upload_images/9047034-6fc6ef4dfcfe3a1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
      总之，记住公式：
      ```
      对象.__proto__ === (对象原型)函数.prototype
      函数.__proto__ === Function.prototype
      Function.prototype.__proto_ === Object.prototype
      函数.prototype.__proto_ === Object.prototype

      // 测试一下代码就可以证明
      function fc(){}

      var o = new fc()
      o.__proto__ === fc.prototype
      fc.__proto__ === Function.prototype
      fc.prototype.__proto__ === Object.prototype
      Object.prototype.__proto__ === null
      ```
      也就是说:
      - 所有对象的`__proto__`引用此对象原型函数的`prototype`属性
      - 所有函数的`__proto__`引用`Function.protype`,不管你自己写的还是 api
      - `Function.protype.__proto__`以及你自己写的函数的共用属性`prototype`的`__proto__`引用`Object.prototype`
      ![](https://camo.githubusercontent.com/b8806bd76878881e7f843b0e77643aff26594ecf/687474703a2f2f3773626e62612e636f6d312e7a302e676c622e636c6f7564646e2e636f6d2f6769746875622d6a732d70726f746f747970652e6a7067)
      [参考](https://github.com/creeperyang/blog/issues/9)
    * 那么，Object.prototype对象有没有它的原型呢？回答是有的，就是没有任何属性和方法的null对象，而null对象没有自己的原型。
    * `Object.getPrototypeOf(Object.prototype)  // null`
      上面代码表示，Object.prototype对象的原型是null，由于null没有任何属性，所以原型链到此为止。
      Object.prototype 就是 Object对象的共有属性
    * [阮一峰](http://javascript.ruanyifeng.com/oop/prototype.html#toc3)