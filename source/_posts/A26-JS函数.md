---
title: A26-JS函数
date: 2018-01-23 22:20:16
tags:
---

## ## Function -[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)
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