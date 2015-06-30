---
layout: post
title: Python XPath库
categories:
- 爬虫
tags:
- 爬虫
- python
- xpath
---

## xpath哲学

	XPath 是一门在 XML 文档中查找信息的语言。XPath 用于在 XML 文档中通过元素和属性进行导航。

1. XPath是一门语言
2. XPath可以在XML文档中查找信息
3. XPath支持HTML
4. XPath通过 元素 和 属性 进行导航

## 安装

	1.安装lxml库
	2.from lxml import etree
	3.Selector = etree.HTML(html)
	4.Selector.xpath(expersion)

## XPath的使用

1. 树状结构
2. 逐层展开
3. 逐层定位
4. 寻找主力结点

Note: 正则匹配要点，先抓大，再抓小

### 如何获得网页中的xpath：

1. 手动分析法
2. Chrome生成法

### 应用XPath提取内容：

	//定位根节点
	/往下层寻找
	提取文本内容：/text()
	提取属性内容：/@xxxx

实例：

	text = html.xpath('//div[@id="url"]/ul/li/@text()')

	