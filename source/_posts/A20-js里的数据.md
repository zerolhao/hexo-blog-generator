---
title: A20-js里的数据
date: 2018-01-07 22:37:40
tags:
---

- ### 非标准的语法
  通过 Babel 来转义使用
  将不存在的语法翻译成存在的语法

- ### js有 7 种数据类型
  1. 数值  number
  2. 字符串  string
  3. 布尔 boolean
  4. Symbol 符号
  5. null 
  6. undefined
  7. 对象 object
  array和function都是对象，除了前面6种其它都是对象

- ### 如何表示这7种数据类型
  #### 1. number
    * 十进制  1、1.1、.1、1.23e2、1.23E2
    * 二进制 0b、0B开头  0b10（十进制2）
    * 八进制 0开头  011（十进制9）
    * 十六进制 0x、0X开头 0x11（十进制17）

  #### 2. string
    - '你好'
    - "你好"
    - ''空字符串
    - ' '空格字符串
    - ''' 转义字符 输出单引号，\ 是转义符
    - 'n'回车 '\t'tab …… 这些都是一个字符
    - '\' 用转义符来转义转义符，输出 \
    - 多行字符串 
      * ```
        var str = '1234\   这个方法抄袭自命令行
        67890' 输出1234567890  这种很容易出现空格但是看不见，很坑
        
      
        var str = '12345' +
        '67890'  // 输出 1234567890，这种方法多达几个字符但是更好读更安全（只推荐这种）
        ES6新增了一种新的语法，使用 反引号（1左边那个按键）
       
       var str = `12345
                  67890`  // 这种不需要加\，但是依然会出现空格，包括缩进产生的
       var str = `12345
       67890`    // 因为有缩进所以依然有空格，并且这种方式保留 回车！
       ```

  #### 3. boolean
    - 布尔其实是一个人的名字，他是一名数学家，他发明了逻辑学
    - true 真
    - false 假
    - 相关：&&（与）、||（或） 运算
      举例： a&&b 
      只有 a 和 b 同时为 真 的时候 a&&b 才为真
      举例： a||b
      只要 a 和 b 有一个为 真，结果就为 真

  #### 4. symbol（暂时用不上，没讲）

  #### 5. null （空对象）
    - 这是一个类型
    - 它只有一个值，就是 null

  #### 6. undefined  （空非对象）
    - 这是一个类型
    - 它只有一个值，就是 undefined

  null 与 undefined 的意思都是 什么也没有，
  它们是 js 的一个bug，是没设计好

  区别
    1. 变量没有赋值 -  值为undefined （语法）
    2. 有一个对象object，现在不想赋值，那么给它一个 null  （惯例）
    （可以给undefined，但是推荐给 null）
    有一个非对象，现在不像赋值，推荐给一个 undefined （不需要自己给，看第一条）

  以上 6 种叫做 基本类型（简单类型，它们就一个值）

  #### 7. object 
    - 它是 复杂类型，由简单类型组成
    - 举例：
      ```
      var person = {
        "name": "zero",  // js 只支持以 字符串 为key
        "age": 20,       // key可以不加引号，js会自动加上
        "gender": "male", // es3不支持最后一个加逗号，es5支持，
                          // ie7及以下不能加，ie8及以上可以加      
      }
      ```
    - 语法：
      - 用`{}`包起来
      - 里面是 键值对
        ```
        {
          key: value,  // key必须是字符串，但是js会自动添加所以不手动添加也可以
          ……           // value 该怎么写就怎么写，
        }
        ```
      - 然后将这个新建的对象赋给一个变量
        ```
        var obj = {
          string: "string",
          number: 10,
          boolean: true,
          obj2:{     // 对象里面还可以有另一个对象，一直嵌套下去
            name:'xxx',
          }
        }
        ```
      - 使用
      `obj['string']`
      `obj.number`
      - 烧脑的地方来了，里面的值可不可以是自己呢？
        ```
        var person = {
          name: 'zero',
          age:'22',
          self: person
        }
        ```
        输出 person.self.self.self.self.name 会是什么结果
        undefined,person中储存的是地址，指向的堆中只有一个对象，
        由于还未将对象赋给person，
        var person;  // 变量提升别忘了
        person = {...} // 先计算对象，然后赋给person地址，因此self的值实际上是undefined，
        即使先赋值再添加self，引用的仍然是同一个对象，self指向的都是同一个地址
      - 空字符串也是字符串，那么空字符串当key可以么？
        ```
        var obj = {
          "": "空字符串"
        }
        ```
        使用：obj[""] 就能输出 "空字符串"
      - 还是key，变量名不可以用数字开头，key可以么
        不加引号的key不可以，不加引号的key默认使用标识符的命名规则
        加引号的key可以，中间加空格也可以，"a b": 'xxx',
      - 很重要的一点
        如果key的书写符合标识符的规则的话
        比如：
        ```
        var person = {
          name: 'zero',
          age: 22
        }
        ```
      可以这样使用：`person.name、person.age   // 这里name、age都是字符串，``
      (特例，只有符合标识符的情况下才可以这样使用)
- ### delete
  可以删除对象的属性，比如
  delete person['name'] （上面那种特里也可以，规则不变）
  person.name // undefined (无value)
  'name' in person  // false (无key)

- ### for in
  - 遍历对象
  - 使用，比如便利上面的person
    ```
    for(var key in person){
      console.log(person[key]) // 不可以用person.key,因为key不是字符串，
    }// 遍历输出的顺序并不一定是像代码那样的顺序
    ```

- ### typeof
  - 用来知道一个变量的类型
  - typeof 返回null时会返回 'object' ,第一个bug
    `typeof null // 'object'`
  - js中只有 7 中类型
  但是typeof 函数的时候却会返回 function， 第二个bug
  `typeof function(){}  // 'function' （数组正常的返回object）``
