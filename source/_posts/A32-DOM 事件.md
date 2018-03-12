---
title: A32-DOM 事件
date: 2018-03-05 21:09:36
tags: [DOM]
categories: 饥人谷
---
# 如何做「点击其他地方关闭浮层」
- 防止冒泡
  w3c的方法是[e.stopPropagation()](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/stopPropagation)，IE则是使用e.cancelBubble = true

- 阻止默认行为
  w3c的方法是[e.preventDefault()](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault)，IE则是使用e.returnValue = false;

```
//  html
<div id="wrapper" class="wrapper">
  <button id="clickMe">点我</button>
  <div id="popover" class="popover">
    <input type="checkbox">浮层
  </div>
</div>
// css
body{border: 1px solid red; }
.wrapper{position: relative; display: inline-block; }
.popover{border: 1px solid red; position: absolute; left: 100%;
  top: 0; white-space: nowrap; padding: 10px; margin-left: 10px;
  background: white; display: none; }
.popover::before{position: absolute; right: 100%; top: 5px;
  border: 10px solid transparent; border-right-color: red;
  content: ''; }
.popover::after{position: absolute; right: 100%; top: 5px;
  border: 10px solid transparent; border-right-color: white;
  content: ''; margin-right: -1px; }
//  js  
//  方案一: 给 document 添加事件，缺陷是如果有很多需要关闭的btn呢？监听事件会一直占着内存
clickMe.addEventListener('click', function(e){
  popover.style.display = 'block'
})
wrapper.addEventListener('click', function(e){
  e.stopPropagation()  // 阻止冒泡，否则冒泡到docunment的监听事件时会执行
})
document.addEventListener('click', function(){
  popover.style.display = 'none'
})
//  方案2：使用jQ，只有点击btn的时候才会对document进行监听，并且只执行一次，
$(clickMe).on('click', function() {
  $(popover).show()
  console.log('show')
  setTimeout(function() {  // 如果不添加setTimeout的话下面的docuemnt监听事件会执行，在执行clickMe的监听事件冒泡的时候，所以我们要“等一会”
    console.log('添加 one click')  // 这里添加了会使得冒泡结束后再执行setTimeout里面的代码
    $(document).one('click', function() {
      console.log('我觉得这里不会执行')
      console.log('hide')
      $(popover).hide()
    })
  }, 0)
})

// 理解DOM事件
<div class="red"> <div class="blue"> <div class="green">
      <div class="yellow"> <div class="orange"> <div class="purple">
          </div> </div> </div> </div> </div>
</div>

*{margin:0;padding:0;box-sizing:border-box;}
.red.active {background: red; }
.blue.active {background: blue; }
.green.active {background: green; }
.yellow.active {background: yellow; }
.orange.active {background: orange; }
.purple.active {background: purple; }
div {border: 1px solid black; padding: 10px;
  transition: all 0.5s; display: flex;
  flex:1; border-radius: 50%; background: white; }
.red{width: 100vw; height: 100vw; }

let divs = $('div')
let n = 0
for (let i = 0; i < divs.length; i++) {
  divs[i].addEventListener('click', () => {
    setTimeout(() => {
      divs[i].classList.add('active')
    }, n * 500)
    n += 1
  }, true)
}
for (let i = 0; i < divs.length; i++) {
  divs[i].addEventListener('click', () => {
    setTimeout(() => {
      divs[i].classList.remove('active')
    }, n * 500)
    n += 1
  })
}
```


# 新 jQuery API
- [.prepend()](https://www.jquery123.com/prepend/)
- [.append()](https://www.jquery123.com/append/)