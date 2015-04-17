---
layout: post
title: jQuery基础教程(第3版)
categories:
- 图灵系列
- 编程语言
tags:
- JavaScript
- 读书笔记
---

## 需要解决的问题

1. 带发掘。

## 第1章 jQuery入门


## 第2章 选择元素

### 本章内容

1. CSS和XPath选择符
2. jQuery独有的自定义选择符
3. jQuery的DOM遍历方法


### $()函数

基本的选择符

|| *选择符* || *CSS* || *jQuery* || *说明* ||
|| 标签名 || p {} || $('p') || 取得文档中所有的段落 ||
|| ID || #some-id {} || $('#some-id') || 取得文档中ID为some-id的一个元素 ||
|| 类 || .some-class {} || #('.some-class') || 取得文档中类为some-class的所有元素 ||


## 第3章 事件

### 本章内容


### 带着问题学习

1. 捕捉滚动鼠标滑轮事件
2. 禁止事件
3. 事件触发原理

### 在页面加载后执行任务

1. 代码执行的时机选择

$(document).ready()

> 什么是加载完成
>   一般来书，使用$(document).ready()要由于使用onload()事件处理程序，但必须要明确一点的是，
>   因为支持文件可能还没有加载完成，所以类似图像的高度和宽度这样的属性此时则还不一定会有效。
>   如果需要访问这些属性，就需要选择实现一个onload事件处理程序($(document).load())。

2. 缩短代码的简写方式

$(docuemnt).ready(function(){
    ...
});

等同于(万恶的...)

$(function(){
    ...
});





## 第5章 操作DOM

### 操作属性

操作class属性的方法：

1. \.addClass() 创建或增加这个属性
2. \.removeClass() 删除或缩短该属性
3. \.toggleClass() 交替的添加和移除一个类