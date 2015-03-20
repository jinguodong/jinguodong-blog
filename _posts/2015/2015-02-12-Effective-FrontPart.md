---
layout: post
title: 提高前段开发效率的那些事(转载)
categories:
- 转载
- 方法论
tags:
- 前段开发
---

转载自：[提高前段开发效率的那些事][1]

[1]: http://www.kuqin.com/shuoit/20150205/344733.html

## 选一个好用的编辑器，并熟练使用她（他）

在我看来好用的编辑器至少有以下功能

1. 语法高亮
2. 括号匹配
3. 能快速定位
 + 快速打开文件
 + 在文件中查找
 + 在文件夹中查找
 + 跳至某一行
4. 能安装插件


## 能熟练用浏览器进行调试
主要包括

1. 审查元素
2. 打断点和条件断点
3. 改变元素的状态，例如：hover,focus
4. 熟练的使用浏览器的控制台


## 确定项目模板

(试用了一下，感觉不太好用)
问题：
1. 是不是这个项目的最大贡献就是解决了index.html的兼容性问题？

推荐在[HTML5 BOILERPLATE](http://www.bootcss.com/p/html5boilerplate/)上做一些自己的定制。
如果是移动端的项目，推荐用MOBILE BOILERPLATE。


## 能快速的生成模板项目
推荐使用Yo。(没找到)


## 创建一个项目的组件页
可以参考bootstrap的。见[这里](http://v3.bootcss.com/components/)


## 积累一些的高质量的常用第三方组件，并自己写一些使用组件的demo
网上总是不缺各种第三方组件。其中不乏很多带很多坑的组件。所以，发现高质量的组件，那就赶紧mark吧。我积累了一些，见[这里](https://github.com/iamjoel/front-end-plugins)。

虽然很多组件都有官方写的demo。但看官方的demo总是需要花一些时间去理解。我的做法是，理解了官方的demo后，自己也写一些demo。那以后再次使用时，就可以看自己写的demo了。


## 积累些常用的代码片段
类似http://css-tricks.com/snippets/。

如果编辑器是用的sublime的话，可以创建代码片段(snippets)。要再用的时候，只需输入几个键，就可以将代码片段输出。


## 积累的一些前段技术文献

1. [W3CPlus](http://www.w3cplus.com/)
2. [10个优秀视差滚动插件](http://www.w3cplus.com/source/10-best-Parallax-Scrolling-plugin.html)


## 其他
还有一些，待探究

项目的依赖管理(bower之类)
预编译（less，coffee之类），后编译（postcss之类）
GTD（Get things done）
最后，推荐一个Chrome插件livestyle。有了它，不用刷新，在chrome中改的css会同步修改到文件，在文件中修改的css也会同步到浏览器。