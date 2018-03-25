---
title: H46-从 MVC 到 MVVM
date: 2018-03-25 18:41:38
tags: [MVVM,Vue.js,Axios]
categories: 饥人谷
---
# 首先来看一段代码
```
<div id="app">
  <div>
    书名：《__name__》
    数量：<span id=number>__number__</span>
    </div>
    <div>
      <button id="addOne">加1</button>
      <button id="minusOne">减1</button>
      <button id="reset">归零</button>
    </div>
</div>
```
从后台获取数据，这次我们使用一个新的库 axios

# 引入[axios](https://github.com/axios/axios)
一个基于 Promise 的 HTTP 客户端，用于浏览器和node.js
1. 比 jQuery.ajax 功能更多
2. 除了 ajax功能之外，没有其它功能
[代码](https://github.com/zerolhao/task-code/blob/master/H46-%E4%BB%8E%20MVC%20%E5%88%B0%20MVVM/%E5%BC%95%E5%85%A5axios.html)

# 引入 MVC
[代码](https://github.com/zerolhao/task-code/blob/master/H46-%E4%BB%8E%20MVC%20%E5%88%B0%20MVVM/%E5%BC%95%E5%85%A5axios%E5%92%8CMVC.html)

# 引入 Vue
[代码](https://github.com/zerolhao/task-code/blob/master/H46-%E4%BB%8E%20MVC%20%E5%88%B0%20MVVM/%E5%BC%95%E5%85%A5vue.html)

## [Vue](https://cn.vuejs.org/)
[教程](https://cn.vuejs.org/)
## 下面是几个用 vue 写的例子
- ## 浮层例子
  ```
  <div id="app">
  </div>
  <script src="https://cdn.bootcss.com/vue/2.5.16/vue.min.js"></script>
  <script>
    let view = new Vue({
      el: '#app',
      data: {
        open: false,
      },
      template: `
        <div>
          <button v-on:click="toggle">点击</button>
          <div v-show="open">现在你看见我了</div>
        </div>
      `,
      methods:{
        toggle(){ // es6 语法，等同于 toggle: function(){
          this.open = !this.open // 注意这里必须有 this
        }
      }
    })
  </script>
  ```
  
- ## 轮播例子
  ```
  // css
  .window{
    width:100px;
    height: 60px;
    margin: auto;
    border: 1px solid black;
  }
  .slides{
    width: 300px;
    height: 60px;
    background-color: #906;
    transition: 1s;
  }
  // html
    <div id="app">
    </div>
    <script src="https://cdn.bootcss.com/vue/2.5.16/vue.min.js"></script>
    <script>
    let view = new Vue({
      el: '#app',
      data: {
        marginLeftNum: 0,
        url:['/a','/b','/c']
      },
      template: `
        <div>
          <div class='window'>
            <div class="slides"
              v-bind:style="{marginLeft: marginLeftNum+'px'}">
              <img v-for="src in url" />
            </div>
          </div>
          <button v-for="(btn, index) in url"
            v-on:click='go(index)'>{{index+1}}
          </button>
        </div>
        `,
      methods: {
        go(n){
          this.marginLeftNum = `${-n*100}`
        }
      }
    })
    </script>
  ```
- ## tab切换例子
  ```
  // css
  .active{
    color: red;
  }
  // html
  <div id="app">
  </div>
  <script src="https://cdn.bootcss.com/vue/2.5.16/vue.min.js"></script>
  <script>
    let view = new Vue({
      el: '#app',
      data: {
        select: 'a',
        tabs: [
          {name:'a',content:'This is A'},
          {name:'b',content:'This is B'},
          {name:'c',content:'This is C'},
        ]
      },
      template: `
        <div>
          <ol>
            <li v-for="tab in tabs"
              v-on:click="select = tab.name"
              v-bind:class="{active:select === tab.name}">
              {{tab.name}}
            </li>
          </ol>
          <ul>
            <li v-for="tab in tabs"
              v-if="select === tab.name">
              {{tab.content}}
            </li>
          </ul>
        </div>
      `
    })
  </script>
  ```

# Questions
- [v-if 与 v-show的区别](https://cn.vuejs.org/v2/guide/conditional.html#v-if-vs-v-show)
使用了v-if的时候，如果值为false，那么页面将不会有这个html标签生成。
v-show则是不管值为true还是false，html元素都会存在，只是CSS中的display显示或隐藏

- 单向绑定与双向绑定
![](https://upload-images.jianshu.io/upload_images/9047034-fee116d672bbe577.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# API
JavaScript
  - [Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
jQuery
  - [.html()](https://www.jquery123.com/html/) 