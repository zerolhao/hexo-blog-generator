---
title: H45-初识webpack
date: 2018-03-24 15:52:32
tags: [webpack,工程化]
categories: 饥人谷
---
# 工程化
  - 自动化（使用sass、babel等工具编译代码）
  - 模块化
  - 性能优化
*自动化工具*
[将scss/sass转为ie也可以用的css](https://github.com/sass/sass)
[使用babel将es6代码转化为es5代码](https://babeljs.io/)
单个使用自动化工具很麻烦，而 webpack 就省事多了，webpack 可以安装插件来实现它们的功能

# [webpack](https://webpack.js.org/)
WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其转换和打包为合适的格式供浏览器使用。

**WebPack和Grunt以及Gulp相比有什么特性**
其实Webpack和另外两个并没有太多的可比性，Gulp/Grunt是一种能够优化前端的开发流程的工具，而WebPack是一种模块化的解决方案，不过Webpack的优点使得Webpack在很多场景下可以替代Gulp/Grunt类的工具。

Grunt和Gulp的工作方式是：在一个配置文件中，指明对某些文件进行类似编译，组合，压缩等任务的具体步骤，工具之后可以自动替你完成这些任务。
![](https://upload-images.jianshu.io/upload_images/9047034-0cfb5fff80435443.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。
![](https://upload-images.jianshu.io/upload_images/9047034-9c4c6d2c1bbf5946.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[使用webpack](https://zhuanlan.zhihu.com/p/30701816)
安装教程，直接看[官网教程](https://webpack.js.org/api/module-methods/)
[面试题](https://www.zhihu.com/question/266788138)

模块化，所有文件都是一个模块，引入到其中一个文件，
运行 `npx webpack` or `npm run build` （注意这是本地安装使用的命令）
然后所有文件会打包放到 bundle.js 里
```
// module1.js
function fn(){
  console.loog(1)
}
export default fn

// module2.js
function fn(){
  console.log(2)
}
export default fn

// app.js
import module1 from './module1'  // 引入模块1
import module2 from './module2'  // 引入模块2
import '../css/style.css' // 引入css
module1()  // use 模块
module2()
```
所有源代码放在 src 文件夹
所有编译后的代码放在 dist 文件夹
src source 未编译
dist distribution 编译后
[代码](https://github.com/zerolhao/task-code/tree/master/H45-webpack-demo)
## webpack 插件
- [Babel Loader](https://github.com/babel/babel-loader)
- [SASS Loader](https://github.com/webpack-contrib/sass-loader)
- [PostCSS Loader](https://github.com/postcss/postcss-loader)

# [parcel](https://parceljs.org/)