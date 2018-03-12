---
title: Hexo+GitHub博客搭建
date: 2017-11-24 20:24:08
tags: [hexo]
categories: hexo 
---
## 如何使用Hexo+GitHub搭建一个博客

首先要声明的是本文默认你已经有git账号并且已经搞定ssh之类。

###### 并且是仅有一个git账户或者本机只有一个git全局账户的情况下！！！（至于为何……因为我还没搞定一套电脑两个账户怎么折腾……）

### 以下是步骤
1. 在github上新建一个空的repo，repo 名称是「你的用户名.github.io」（请将你的用户名替换成真正的用户名）
2. 进入一个安全的目录，你可以在D盘或者其它某个磁盘下创建一个文件夹，然后进入这个文件夹，比如我是D盘：
打开git bash
```
cd d:
mkdir myblog
cd myblog
```
3. 然后安装hexo
`npm install -g hexo-cli`
4. 初始化，`hexo init`
5. 安装（我也不知道安装什么），`npm i`
6. `hexo new 开博大吉`,你会看见一个md文件的路径
7. `start xxxxxxxxx.md`,编辑这个md文件，你也可以手动去翻文件夹然后编辑
8. `start _config.yml`,编辑网站配置
  1. 把第六行的title改成你想要的名字
  2. 把第九行的author改成你的名字
  3. 把最后一行的type改成type: git
  4. 在最后一行后面新增一行，左边与 type 平齐，加上一行`repo: 仓库地址`
     请将仓库地址改为「你的用户名.github.io」对应的仓库地址，仓库地址以 git@github.com: 开头
9. `npm install hexo-deployer-git --save`,安装git部署插件
10. `hexo deploy`
11. 进入「你的用户名.github.io」对应的 repo，打开 GitHub Pages 功能，默认应该是打开的，如果是，就直接点击预览
12. 你现在应该看到了你的博客!

### 第二篇博客
1. `hexo new 第二篇博客`
2. 复制显示路径。使用`start 路径`来编辑它
3. `hexo generate`
4. `hexo deploy`
5. 去看看你的博客，应该能看见第二篇博客了

### 换主题
1. [主题](https://github.com/hexojs/hexo/wiki/Themes) 这是一个主题合集
2. 随便找一个主题合集，进入主题的github首页,比如我找的是
[git@github.com:xiangming/landscape-plus.git](git@github.com:xiangming/landscape-plus.git)
3. 复制它的 SSH 地址或 HTTPS 地址，假设地址为 git@github.com:iissnan/hexo-theme-next.git
4. `cd themes`
5. `git clone git@github.com:iissnan/hexo-theme-next.git`
6. `cd ..`
7. 将 _config.yml 的第 75 行改为 `theme: landscape-plus.git`,保存
8. `hexo g`
9. `hexo d`
10. 等上一会，然后刷新你的博客页面，你就会看到一个新的外观。如果不喜欢这个主题，就回到第一步，重选一个主题再来一遍。

###### 以上就是使用Hexo配合GitHub搭建个人博客的教程了。