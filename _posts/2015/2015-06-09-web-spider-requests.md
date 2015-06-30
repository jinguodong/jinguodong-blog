---
layout: post
title: Python Requests库：HTTP for Humans，pythonrequests
categories:
- 爬虫
tags:
- 爬虫
- python
- Requests
---

## Requests哲学

Requests 支持 HTTP 连接保持和连接池，支持使用 cookie 保持会话，支持文件上传，支持自动确定响应内容的编码，支持国际化的 URL 和 POST 数据自动编码。现代、国际化、人性化。

## Quickstart

Making  a request with Requests is very simple

    import requests
    r = requests.get('https://api.github.com/events')
    