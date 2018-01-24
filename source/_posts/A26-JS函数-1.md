---
title: A26-JS函数
date: 2018-01-24 23:34:02
tags:
---

## Function -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)
  ```
  var f = function(a,b){
    return a+b
  }
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

## 函数的5种声明方式
function 有一个属性：`name`
1. 具名函数
```
function f(x,y){
  console.log(x+y) // 打印什么和返回什么没有必然联系
  return undefined // 必须有一个return，没有的话也会被自动添加本句
}
f.name // 'f'
```
2. 匿名函数
它不能单独使用，可以赋给一个变量来使用[函数表达式](https://developer.mozilla.org/zh-CN/docs/web/JavaScript/Reference/Operators/function)
```
 var f
 f = function(x,y){
     return x+y
 }
 f.name // 'f'
```
3. 具名函数赋值 -[阮一峰](http://javascript.ruanyifeng.com/grammar/function.html#toc1)
```
function f(){}
f.name // 'f'
console.log(f) // 'function f(){}'

var f2 = function fn(){}
f2.name // 'fn'
console.log(fn) // Uncaught ReferenceError: fn is not defined
// 只能在 function fn(){}这个函数里面访问到fn，在外面无效
// js 的不一致性
```
4. window.Function 函数对象
```
var f = new FUnction('x', 'y', 'return x+y')
f.name // 'anonymous'

var n=1
var fn = new Function('x', 'y', 'return x+' + n + '+y' ) 
console.log(fn) // f anonymous(x,y){ return x+1+y }
fn(1,2) // 4
```
5. 箭头函数 -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
```
var f = (x,y) => {return x+y}
f.name // 'f'

var f2 = (x,y) => x+y // 只有一句 return 的时候，可以将 return 和 {} 省略
var f3 = n => n*n // 如果参数只有一个，() 可以省略
```

## 函数的本质
函数是一段可以反复调用的代码块。
函数还能接受输入的参数，不同的参数会返回不同的值。
函数和数据不同，不能直接使用，只能调用
```
// 定义一个 n
var n=2
// 然后直接使用它就好了
var m = n // 比如将它赋值给 m
console.log(n) // 或者打印出来

// 定义一个函数
function f(x,y){
  return x+y
}
f  // 打印出 f 函数，等于啥也没做，return x+y 并没有执行
// 函数只能调用（call）
f(1,2) // 3
```
### 函数是怎样储存的
![](http://upload-images.jianshu.io/upload_images/9047034-ee70f4d296ed3fca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
函数也是对象的一种，在stack中储存地址，在heap中储存对象，保存的是字符串（就是你定义的函数，参数、函数体等），通过Function.prototype里的call()来调用函数体
我们来试着用对象来做一个这样的函数
```
var f = {}
f.name = 'f' // 函数本身就有的属性
f.params = ['x','y']  // 模拟参数
f.functionBody = 'console.log(1)'  // 模拟函数体，不管参数还是函数体，都是字符串形式
f.call = function(){
  return window.eval(f.functionBody)
}
f.call() 
// 1
// undefined， console.log()的返回值
// 这就是为什么说函数是对象，函数调用实际上就是eval()函数的函数体的过程
// f与f.call()区别：f是变量，是函数（这里用的是对象），f.call()是调用函数
```
**所以函数就是可以执行代码的对象**
eval() -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval)
eval()函数会将传入的字符串当做JavaScript代码进行执行。
### f(1,2) 与 f.call(undefined, 1, 2)
![](http://upload-images.jianshu.io/upload_images/9047034-9bdd51ac634c5f4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
前者是语法糖，后者才是真正的写法
理解这两者的区别才能更好的理解this
[SegmentFault](https://segmentfault.com/a/1190000011819236)
### call()怎么用 -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
```
function f(x,y){ return x+y }
f.call(undefined, 1, 2) // 3
```
call() 方法调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。
语法：`fun.call(thisArg, arg1, arg2, ...)`
参数：
thisArg
在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，
如果这个函数处于**非严格模式**下，则指定为null和undefined的this值会自动指向全局对象(浏览器中就是window对象)，
同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。
在严格模式下，this将保持他进入执行上下文时的值，所以下面的this将会默认为undefined。(详见this-mdn)
arg1, arg2, ...
指定的参数列表。
返回值：返回值是你调用的方法的返回值，若该方法没有返回值，则返回undefined。
描述：
可以让call()中的对象调用当前对象所拥有的function。
你可以使用call()来实现继承： 写一个方法，然后让另外一个新的对象来继承它（而不是在新对象中再写一次这个方法）。
### this -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)-[阮一峰](http://javascript.ruanyifeng.com/oop/this.html) 与 arguments [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/arguments)
什么是this，在以下代码中
`f.call(undefined, 1, 2)`
`undefined`就是`this`
`1`和`2`就是`arguments`
`arguments`是伪数组别忘了
或者说：
call的第一个参数可以用this得到
call的后面所有参数可以用arguments得到
```
// Eg
var f = function(){
  console.log(this)
  console.log(this === window)
  console.log(arguments)
}
f.call(undefined, 1,2,3)
// log this -> window 对象 原因见上文-call()怎么用
// true
// log arguments -> [1,2,3] 还有一些属性api见mdn

var f2 = function(){
  'use strict'  // use 严格模式
  console.log(this)
  console.log(this === window)
  console.log(arguments)
}
f2.call(undefined)
// undefined
// false
// []

f2.call(7)
// 7 false []
 f2.call('foo')
 // 'foo' false []

var f3 =  function() {
  console.log(Object.prototype.toString.call(this));
}
//原始值 1 被隐式转换为对象
f3.call(1); // [object Number]
```
**注意:** 使用 call 和 apply 函数的时候，如果传递的 this 值不是一个对象，JavaScript 将尝试使用内部 ToObject 操作将其转换为对象。因此，如果传递的值是一个原始值比如 7 或 'foo'，那么就会使用相关构造函数将它转换为对象，所以原始值 7 通过new Number(7)被转换为对象，而字符串'foo'使用 new String('foo') 转化为对象。（这一点上文也有描述）
this 也是为了长得像 java 而诞生的，和 new 一样

## call stack 调用栈
表示函数或子例程像堆积木一样存放，以实现层层调用。
别忘了栈是先入后出。
![嵌套调用](http://upload-images.jianshu.io/upload_images/9047034-85dfa91ff6fc6193.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- [普通调用](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gYSgpewogICAgY29uc29sZS5sb2coJ2EnKQogIHJldHVybiAnYScgIAp9CgpmdW5jdGlvbiBiKCl7CiAgICBjb25zb2xlLmxvZygnYicpCiAgICByZXR1cm4gJ2InCn0KCmZ1bmN0aW9uIGMoKXsKICAgIGNvbnNvbGUubG9nKCdjJykKICAgIHJldHVybiAnYycKfQoKYS5jYWxsKCkKYi5jYWxsKCkKYy5jYWxsKCk%3D!!!)
- [嵌套调用](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gYSgpewogICAgY29uc29sZS5sb2coJ2ExJykKICAgIGIuY2FsbCgpCiAgICBjb25zb2xlLmxvZygnYTInKQogIHJldHVybiAnYScgIAp9CmZ1bmN0aW9uIGIoKXsKICAgIGNvbnNvbGUubG9nKCdiMScpCiAgICBjLmNhbGwoKQogICAgY29uc29sZS5sb2coJ2IyJykKICAgIHJldHVybiAnYicKfQpmdW5jdGlvbiBjKCl7CiAgICBjb25zb2xlLmxvZygnYycpCiAgICByZXR1cm4gJ2MnCn0KYS5jYWxsKCkKY29uc29sZS5sb2coJ2VuZCcp!!!)
- [递归调用](http://latentflip.com/loupe/?code=ZnVuY3Rpb24gc3VtKG4pewogICAgaWYobj09MSl7CiAgICAgICAgcmV0dXJuIDEKICAgIH1lbHNlewogICAgICAgIHJldHVybiBuICsgc3VtLmNhbGwodW5kZWZpbmVkLCBuLTEpCiAgICB9Cn0KCnN1bS5jYWxsKHVuZGVmaW5lZCw1KQ%3D%3D!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
stack overflow 栈溢出
call stack 超过电脑分配空间就会出现
```
function sum(n){
    if(n==1){
        return 1
    }else{
        return n + sum.call(undefined, n-1)
    }
}

sum.call(undefined,5) //15

sum.call(undefined, 100000)
// Uncaught RangeError: Maximum call stack size exceeded
// 最大调用栈大小超过
```
- [call stack-mdn](https://developer.mozilla.org/zh-CN/docs/Glossary/Call_stack)
- [call stack-阮一峰](http://www.ruanyifeng.com/blog/2013/11/stack.html)
- [stack overflow-wiki](https://www.wikiwand.com/zh-hans/%E5%A0%86%E7%96%8A%E6%BA%A2%E4%BD%8D)
- [stackoverflow.com](https://stackoverflow.com/)捏他的同名程序问答网站
- [国内版stackoverflow->SegmentFault](https://segmentfault.com/)

## 作用域 -[MDN](https://developer.mozilla.org/zh-CN/docs/Glossary/Scope)-[阮一峰](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
- 按照语法树，就近原则
- 我们只能确定变量是哪个变量，但是不能确定变量的值
### 面试题
拿到代码直接做——必然会错。请先提升声明
```
// 1.
var a = 1
function f1(){
    alert(a) // 是多少
    var a = 2
}
f1.call() // undefined

// 2.
var a = 1
function f1(){
    var a = 2
    f2.call()
}
function f2(){
    console.log(a) // 是多少
}
f1.call() // 1

// 3.
<ul>
  <li>1</li> <li>2</li> <li>3</li> <li>4</li> <li>5</li> <li>6</li>
</ul>
var liTags = document.querySelectorAll('li')
for(var i = 0; i < liTags.length; i++){
    liTags[i].onclick = function(){
        console.log(i) // 点击第3个 li 时，打印 2 还是打印 6？
    }
}
// 打印 6
// 当你点击时for早就遍历完，i 已经自增到 6
```
## 闭包 -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)-[方应杭](https://zhuanlan.zhihu.com/p/22486908)
