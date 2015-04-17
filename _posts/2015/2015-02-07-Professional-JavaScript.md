---
layout: post
title: JavaScript高级程序设计（第三版）
categories:
- 图灵系列
- 编程语言
tags:
- JavaScript
- 读书笔记
---

## 需要解决的问题

1. 鼠标滚动事件处理。
2. 图片块区滚动效果处理。
3. JavaScript实现网站模块式开发。

## 第一章 JavaScript简介

### DOM级别

DOM1级：

    包括DOM核心（DOM Core）和DOM HTML两个模块。其中DOM核心规定的是如何映射基于XML的文档结构，一边对文档中任意部分的访问和操作。DOM HTML模块则在DOM核心的基础上加以扩展，添加了针对HTML的对象和方法。

DOM2级：

    DOM2级在原来的基础上又扩充了鼠标和用户界面事件、范围、遍历（迭代DOM文档的方法）等细分模块，而且通过对象接口增加了对CSS的支持。

DOM2级引入了下列新模块：

> 1. DOM视图（DOM Views）：定义了跟踪不同文档（例如，应用CSS之前和之后的文档？）视图的借口。
> 2. DOM事件（DOM Events）：定义了事件和事件处理的接口。
> 3. DOM样式（DOM Style）：定义了基于CSS为元素应用样式的接口。
> 4. DOM遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口。

DOM3级：

    进一步扩展了DOM，引入了以统一方式加载和保存文档的方法---在DOM加载和保存（DOM Load and Save）模块中定义；新增验证文档的方法---在DOM验证（DOM Validation）模块中定义）等。

### 浏览器对象模型（BOM）

    根本上讲，BOM只处理浏览器窗口和框架；但人们习惯把所有针对浏览器的JS扩展都算做BOM的一部分。

BOM的一些扩展功能：

1. 弹出新浏览器窗口的功能；
2. 移动、缩放和关闭浏览器窗口的功能；
3. 提供浏览器详细信息的navigator对象；
4. 提供浏览器所加载页面下的详细信息的location对象；
5. 提供用户显示器分辨率详细信息的screen对象；
6. 对cookies的支持；
7. 想XMLHttpRequest和IE的ActiveXObject这样的自定义对象。

### 小结（2015-02-07）

JavaScript有以下三个不同的部分组成：

1. ECMAScript，提供核心语言功能；
2. 文档对象模型DOM，提供访问和操作页面内容的方法和接口；
3. 浏览器对象模型，提供与浏览器交互的方法和接口。


## 第二章 在HTML中使用JavaScript

    <script type="text/javascript" src="jjj.js"></script>

    ？？？后续补充


## 第三章 基本概念

### 本章内容

1. 语法
2. 数据类型
3. 流控制语句
4. 函数

### 带着问题学习

1. 函数的定义：

    基本形式：
    function funcName(arg1, arg2){
        statements
    }
    理解参数：
    1. 函数体内可以通过arguments对象来访问这个参数数组，从而获取传递给函数的每一个参数
    2. 没有传递值的命名参数将自动被赋予undefined值（定义了变量但是没有初始化）。

2. for in 循环的使用：

    for-in是一种精准的迭代语句，用来枚举对象的属性。
    for (var param_name in expression){
        statements;
    }

    ？？？后续补充


## 第四章 变量、作用域和内存问题

### 本章内容

1. 理解基本类型和引用类型的值
2. 理解执行环境
3. 理解垃圾收集

    ？？？后续补充


## 第五章 引用类型

### 本章内容

1. 使用对象
2. 创建并操作数组
3. 理解基本的JS类型
4. 使用基本类型和基本包装类型

    ？？？后续补充


## 第六章 面向对象的程序设计

### 本章内容

1. 理解对象属性
2. 理解并创建对象
3. 理解继承

    ？？？后续补充


## 第七章 函数表达式

### 本章内容

1. 函数表达式的特征
2. 使用函数实现递归
3. 使用闭包定义私有变量

    ？？？ 后续补充


## 第八章 BOM

### 本章内容

1. 理解window对象————BOM的核心
2. 控制窗口、框架和弹出窗口
3. 利用location对象中的页面信息
4. 使用navigator对象了解浏览器

    ？？？后续补充


## 第九章 客户端检测

### 本章内容

1. 使用能力检测
2. 用户代理检测的历史
3. 选择检测方式

    ？？？后续补充


## 第十章 DOM

### 本章内容

1. 理解包含不同层次节点的DOM
2. 使用不同的节点类型
3. 克服浏览器兼容性问题及各种陷阱

    ？？？后续补充


## 第十一章 DOM扩展

### 本章内容

1. 理解Selectors API
2. 使用HTML5 DOM扩展
3. 使用专有的DOM扩展

    ？？？马上补充


## 第十二章 DOM2和DOM3

### 本章内容

1. DOM2和DOM3的变化
2. 操作样式的DOM API
3. DOM遍历与范围

    ？？？后续补充


## 第十三章 事件

### 本章内容

1. 理解事件流
2. 使用事件处理程序
3. 不同的事件类型

### 事件流

    事件流概念分为两种：事件冒泡流（被普遍支持）；事件捕获流。

#### DOM事件流

![][pic1]

[pic1]: http://www.17jquery.com/uploads/allimg/c110106/12942Y5150Z-550T.jpg

### 事件处理程序

#### HTML事件处理程序：

    <input type="button" value="Click Me" on click="alert('Clicked')" />
    由于onclick特性将JavaScript代码作为其值来定义的。因此不能在其中使用未经转义的HTML语法字符，如：&, "", <, >。

这样指定事件处理程序具有一些独到之处。首先，会创建一个封装着元素属性值的函数。这个函数中有一个局部变量event，也就是事件对象。

    <input type="button" value="click me", onclick="alert(event.type)"/>

在这个函数内部，this值等于事件的目标元素。

关于这个动态创建的函数，另一个有意思的地方是它扩展作用域的方式。


    function(){
        with(document){
            with(this){

            }
        }
    }
    <input type="button" value="click me", onclick="alert(value)"/>

缺点：有两个。
+ 存在时差问题。

    通过try-catch块解决。
    <input type="button" value="click me", onclick="try{showMsg();}catch(ex){}"/>

+ 这样扩展事件处理程序的作用域链在不同浏览器中会导致不同结果
+ 通过HTML指定事件处理程序的最后一个缺点是HTML与JavaScript代码紧密耦合。

#### DOM0级事件处理程序

通过JavaScript指定时间处理程序的传统方式，将一个函数赋值给一个事件处理程序属性。

    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
        alter("Clicked");
    }

使用DOM0级方法指定的事件处理程序被认为是元素的方法。因此，这时候的事件处理程序是在元素的作用域中运行，程序中的this引用当前元素。

    var btn = document.getElementById("myBtn");
    btn.onclick = function(){
        alter(this.id);
    }

也可以删除DOM0级方法指定的事件处理程序：
    btn.onclick = null;

#### DOM2级事件处理程序

提供两个方法：用于指定和删除事件处理程序的操作，addEventListener()和removeEventListener()。所有DOM节点都包含这两个方法，并且它们都接受3个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后的这个布尔值如果为true，表示在捕获阶段调用事件处理程序；如果是false，表示在冒泡阶段调用事件处理程序。

通过addEventListener()添加的事件处理程序只能使用removeEventListener()来移除；

大多数情况下，布尔值应设置为false，最大限度的兼容各种浏览器。

    IE9, Firefox, Safari, Chrome和Opera支持DOM2级事件处理程序。


#### IE事件处理程序

IE实现了DOM中类似的两个方法，attachEvent()和detachEvent()。这两个方法接受相同的两个参数：事件处理程序名称与事件处理程序函数。由于IE8及更早的版本只支持事件冒泡，所以通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。

    var btn = document.getElementById("myBtn");
    btn.attachEvent("onclick", function(){
        alter(this.id);
    });

在IE中使用attachEvent()与使用DOM0级方法的主要区别在于事件处理程序的作用域。

    在使用attachEvent()方法的情况下，事件处理程序会在全局作用域中进行，因此this和window相同。

    addEventListener()与attachEvent()类似，可以用来为一个元素添加多个事件处理程序。但是attachEvent()以添加它们的相反的顺序执行。


#### 跨浏览器的事件处理程序

要确保处理事件的代码能在大多数浏览器下一致的运行，只需要关注冒泡阶段。

    var EventUtil= {
        addHandler: function(element, type, handler){
            if(element.addEventListener){
                element.addEventLister(type, handler, false);
            }else if(element.attachEvent){
                element.attachEvent("on" + type, handler);
            }else{
                element["on"+type] = handler;
            }
        },
        removeHandler: function(element, type, handler){
            if(element.removeEventListener){
                element.removeEventListener(type, handler, false);
            }else if(element.detachEvent){
                element.detachEvent("on" + type, handler);
            }else{
                element["on" + type] = null;
            }
        }
    }

注意： DOM0级对每个事件只支持一个事件处理程序。


### 事件对象

> 在触发DOM上的某个事件时，会产生一个事件对象event，这个对象包含着所有与事件有关的信息。
> 包括事件的元素、事件的类型以及其他与特定事件相关的信息。

#### DOM中的事件对象

|属性方法|类型|读/写|说明|
|:------:|:--:|:---:|:--:|
|bubbles|Boolean|只读|表明事件是否冒泡|
|currentTarget|Element|只读|其事件处理程序当前正在处理事件的那个元素|
|preventDefault()|Function|只读|取消事件的默认行为。如果cancelable是true，则可以使用这个方法|
|stopPropagation()|Function|只读|取消事件的进一步捕获或冒泡，同时阻止任何事件处理程序被调用（DOM3级事件中新增）|
|target|Element|只读|事件的目标|
...待添加

在事件处理程序内部，对象this始终等于currentTarget的值，而target则只包含事件的实际目标。

如果需要通过一个函数处理多个事件时，可以通过type属性。

    var btn = document.getElementById("myBtn");
    var handler = function(event){
        switch(event.type){
            case "click":
                break;
            case "mouseover":
                break;
            case "mouseout":
                break;
        }
    };
    btn.onclick = handler;
    btn.onmouseover = handler;
    btn.onmouseout = handler;

> 要阻止特定事件的默认行为，可以重用preventDefault()方法。例如，链接的默认行为就是在被单击时会导航到其href特定指定的URL。

> 只有cancelable是行为true是，才可以使用preventDefault()来取消其默认行为。

> 另外，stopPropagation()方法用于立即停止事件在DOM层次中的传播。


#### IE中的事件对象

待添加


#### 跨浏览器的事件对象

    var EventUtil = {
        addHandler: function(element, type, handler){
            ...
        },
        getEvent: function(event){
            return event? event: window.event;
        },
        getTarget: function(event){
            return event.target || event.srcElement;
        },
        preventDefault: function(event){
            if(event.preventDefault){
                event.preventDefault();
            }else{
                event.returnValue = false;
            }
        },

        removeHandler: function(element, type, handler){
            ...
        },

        stopPropagation: function(event){
            if(event.stopPropagation){
                event.stopPropagation();
            }else{
                event.cancelBubble = true;
            }
        }
    };


### 事件类型

类型归纳：

1. UE事件，当用户与页面上的元素交互式触发；
2. 焦点事件，当元素获得或失去焦点时触发；
3. 鼠标事件，当用户通过鼠标在页面上执行操作时触发；
4. 滚轮事件，当使用鼠标滚轮时触发；
5. 文本事件，当在文档中输入文本时触发；
6. 键盘事件，当用户通过键盘在页面上执行操作时触发；
7. 合成事件，
8. 变动事件，当底层DOM结构发生变化时触发。


#### UI事件




### 内存和性能


### 模拟事件


### 小结


### 带着问题学习

1. 利用鼠标的滚动事件实现整页滚动效果。
2. iscroll插件，内部元素拖拽影响到外部元素的拖拽，禁止拖拽事件外传？
2. [10个优秀视差滚动插件][link2]

[link2]: http://www.w3cplus.com/source/10-best-Parallax-Scrolling-plugin.html





## 表单脚本


## 使用Canvas绘图


## HTML5脚本编程


## 错误处理与调试


## JavaScript与DML



