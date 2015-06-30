---
layout: post
title: Ubuntu下环境搭建
categories:
- Ubuntu
tags:
- Ubuntu
---

## 安装LAMP

首先下载安装apache2
输入：sudo apt-get install apache2

安装完毕后，在浏览器中输入：localhost
显示如下图，说明安装正确。

紧接着安装php5
输入：sudo apt-get install php5 

安装完毕后，再安装MySQL
输入：sudo apt-get install mysql-server

紧接着安装phpmyadmin
输入：sudo apt-get install phpmyadmin
根据提示选择，选择apache2 再输入root密码 和数据库密码

紧接着改写 /var/www目录的权限。方便日后编辑网站文件。
输入：sudo chmod 777 /var/www

最后创建phpmyadmin链接。
输入：sudo ln -s /usr/share/phpmyadmin /var/www/html/

现在在浏览器中输入：localhost/phpadmin
登陆后就能正确显示管理界面。

