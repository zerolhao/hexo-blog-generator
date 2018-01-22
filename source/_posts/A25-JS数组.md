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
  - `forEach`API -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)[阮一峰](http://javascript.ruanyifeng.com/stdlib/array.html#toc15)
    forEach() 方法对数组的每个元素执行一次提供的函数。
    语法：
    ```
    array.forEach(callback(currentValue, index, array){
        //do something
    }, this)
    
    array.forEach(callback[, thisArg])

    // Eg
    const arr = ['a', 'b', 'c'];

    arr.forEach(function(element) {
        console.log(element);
    });  
    arr.forEach( element => console.log(element));
    // a
    // b
    // c

    // 上面我们并没有将arr传进forEach里面但是为什么还能调用
    // 可以这么理解
    // 用this来获取，比如
    var obj = { 0:'a', 1:'b', length:2 }
    obj.forEach = function(fn){
      for(let i=0; i < this.length; i++){
        fn(this[i], i, this)
      }
    }
    ```
    参数：
    `callback`
    为数组中每个元素执行的函数，该函数接收三个参数：
    `currentValue`(当前值)
    数组中正在处理的当前元素。
    `index`(索引)
    数组中正在处理的当前元素的索引。
    `array`
    `forEach()`方法正在操作的数组。
    `thisArg`可选
    可选参数。当执行回调 函数时用作this的值(参考对象)。
    返回值：`undefined`.
  - `sort()` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)[阮一峰](http://javascript.ruanyifeng.com/stdlib/array.html#toc13)
    sort方法对数组成员进行排序，默认排序顺序是根据字符串Unicode码点。 sort 排序不一定是稳定的排序后，原数组将被改变。
    ```
    ['d', 'c', 'b', 'a'].sort()
    // ['a', 'b', 'c', 'd']
    
    [4, 3, 2, 1].sort()
    // [1, 2, 3, 4]
    
    var fruit = ['cherries', 'apples', 'bananas'];
    fruit.sort(); 
    // ['apples', 'bananas', 'cherries']
    
    var scores = [1, 10, 21, 2]; 
    scores.sort(); 
    // [1, 10, 2, 21]
    // 注意10在2之前,
    // 因为在 Unicode 指针顺序中"10"在"2"之前
    
    var things = ['word', 'Word', '1 Word', '2 Words'];
    things.sort(); 
    // ['1 Word', '2 Words', 'Word', 'word']
    // 在Unicode中, 数字在大写字母之前,
    // 大写字母在小写字母之前.
    ```
    语法：
    `arr.sort()`    
    `arr.sort(compareFunction)`
    参数:
    `compareFunction`
    可选。用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。
    返回值: 返回排序后的数组。原数组已经被排序后的数组代替。
    描述：
    如果没有指明 `compareFunction` ，那么元素会按照转换为的字符串的诸个字符的 Unicode 位点进行排序。例如 "Banana" 会被排列到 "cherry" 之前。当数字按由小到大排序时，9 出现在 80 之前，但因为（没有指明 `compareFunction`），比较的数字会先被转换为字符串，所以在Unicode顺序上 "80" 要比 "9" 要靠前。
    如果指明了 `compareFunction` ，那么数组会按照调用该函数的返回值排序。即 a 和 b 是两个将要被比较的元素：
      + 如果 `compareFunction(a, b)` 小于 0 ，那么 a 会被排列到 b 之前；
      + 如果 `compareFunction(a, b)` 大于 0 ， b 会被排列到 a 之前
      + 如果 `compareFunction(a, b)` 等于 0 ， a 和 b 的相对位置不变
      + `compareFunction(a, b)` 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。
    ```
    // 对数字排序
    var a = [5,6,4,1,2,3]
    // 升序
    function up(a, b) {
      if (a < b ) {           // 按某种排序标准进行比较, a 小于 b
        return -1;
      }
      if (a > b ) {
        return 1;
      }
      // a must be equal to b
      return 0;
    }
    // 更简洁的写法
    function up(a,b){
      return a-b
    }
    a.sort(up) // 1,2,3,4,5,6

    // 降序
    function down(a,b){
      return b-a
    }
    ```
    js内置的排序一般都是快排
  - `join()` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
    join() 方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。
    和sort()方法不同，它不会改变原有的数组
    语法:
    ```
    str = arr.join()
    // 默认为 ","
    
    str = arr.join("")
    // 分隔符 === 空字符串 ""
    
    str = arr.join(separator)
    // 分隔符
    ```
    参数:
    `separator`
    指定一个字符串来分隔数组的每个元素。
    如果需要(`separator`)，将分隔符转换为字符串。
    如果省略()，数组元素用逗号分隔。默认为 ","。
    如果separator是空字符串("")，则所有元素之间都没有任何字符。
    返回值
    一个所有数组元素连接的字符串。如果 arr.length 为0，则返回空字符串
    ```
    // Eg
    let a = ['Wind', 'Rain', 'Fire'];

    console.log(a.join()); 
    // 默认为 ","
    // 'Wind,Rain,Fire'
    
    console.log(a.join("")); 
    // 分隔符 === 空字符串 ""
    // "WindRainFire"
    
    console.log(a.join("-")); 
    // 分隔符 "-"
    // 'Wind-Rain-Fire'
    ```
  - `concat()` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)
    concat() 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。
    语法:
    `var new_array = old_array.concat(value1[, value2[, ...[, valueN]]])`
    参数：valueN
    将数组和/或值连接成新数组。
    返回值：新的 Array 实例。
    ```
    var arr1 = ['a', 'b', 'c'];
    var arr2 = ['d', 'e', 'f'];
    
    var arr3 = arr1.concat(arr2);
    
    // arr3 is a new array [ "a", "b", "c", "d", "e", "f" ]

    // 使用 concat 来复制一个数组
    var a = [1,2,3]
    bar b = a.concat([])
    a === b // false
    ```
  - `map()` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)[阮一峰](http://javascript.ruanyifeng.com/stdlib/array.html#toc14)
    map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。
    map与forEach类似，不同的是它返回一个新数组，而forEach返回undefined。
    语法:
    ```
    let new_array = arr.map(function callback(currentValue, index, array) { 
        // Return element for new_array 
    }[, thisArg])

    // Eg
    var numbers = [1, 2, 3];

    numbers.map(function (n) {
      return n + 1;
    });
    // [2, 3, 4]
    
    numbers
    // [1, 2, 3]
    ```
    参数:
    `callback`
    生成新数组元素的函数，使用三个参数：
    `currentValue`
    callback 的第一个参数，数组中正在处理的当前元素。
    `index`
    callback 的第二个参数，数组中正在处理的当前元素的索引。
    `array`
    callback 的第三个参数，map 方法被调用的数组。
    `thisArg`
    可选的。执行 callback 函数时 使用的this 值。
    返回值:
    一个新数组，每个元素都是回调函数的结果。
  - `filter()` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
    filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。 
    语法:
    `var new_array = arr.filter(callback[, thisArg])`
    返回值: 一个新的通过测试的元素的集合的数组
    ES6:
    let [...spread]= [12, 5, 8, 130, 44];
    等同于：let spread = 浅克隆([12, 5, 8, 130, 44]) 
    ```
    function isBigEnough(value) {
      return value >= 10;
    }
    
    var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
    
    // filtered is [12, 130, 44]
    
    // ES6 way
    
    const isBigEnough = value => value >= 10; // 这里用的箭头函数
    
    let [...spread]= [12, 5, 8, 130, 44];
    
    let filtered = spread.filter(isBigEnough);
    
    // filtered is [12, 130, 44]
    ```
  - `reduce()` -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
    reduce() 方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。
    语法：`arr.reduce(callback(accumulator,currentValue)[, initialValue])`
    参数
    `callback`
    执行数组中每个值的函数，包含四个参数：
      `accumulator`
      累加器累加回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（如下所示）。
      
      `currentValue`
      数组中正在处理的元素。
      `currentIndex`
      数组中正在处理的当前元素的索引。 如果提供了initialValue，则索引号为0，否则为索引为1。
      `array`
      调用reduce的数组
    `initialValue`
    可选,用作第一个调用callback的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。在没有初始值的空数组上调用 reduce 将报错。
    返回值: 函数累计处理的结果
    ```
    var total = [0, 1, 2, 3].reduce(function(sum, value) {
      return sum + value;
    }, 0);
    // total is 6
    
    var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
      return a.concat(b);
    }, []);
    // flattened is [0, 1, 2, 3, 4, 5]

    // 使用 reduce 表示 map
    var a = [1,2,3]
    a.map(function(value){
      return value * 2
    })

    a.reduce(function(arr, n){
      arr.push(n*2)
      return arr
    }, [])

    // 使用 reduce 表示 filter
    var b = [1,2,3,4,5,6,7,8,9,10]
    b.filter(function(value){
      return value % 2 === 0
    })

    b.reduce(function(arr, n){
      if(n % 2 === 0){
        arr.push(n)
      }
      reruen arr
    },[])
    ```


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