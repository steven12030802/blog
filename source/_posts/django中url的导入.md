---
title: django中url的导入
sidebar:
  - toc
date: 2023-01-06 09:31:20
tags: [django, url]
---
### 报错代码：
```
cannot import name ‘url‘ from ‘django.conf.urls‘
```
### 问题原因:
django已经把django.conf.urls.url() 废弃掉了

### 解决办法:
```
from django.urls import re_path as url
```
替换
```
from django.conf.urls import url
```