---
title: docker自动启动和容器自动启动
date: 2022-09-28 10:33:44
tags: docker
sidebar: [toc]
---
1. docker服务启动启动
```
# 开启 docker 自启动
systemctl enable docker.service

# 关闭 docker 自启动
systemctl disable docker.service
```
2. docker 容器自动启动
```
# 开启容器自启动
docker update --restart=always 【容器名】
例如：docker update --restart=always tracker


# 关闭容器自启动
docker update --restart=no【容器名】
例如：docker update --restart=no tracker

##### 相关配置解析
no：
    不要自动重启容器。（默认）

on-failure： 
    如果容器由于错误而退出，则重新启动容器，该错误表现为非零退出代码。

always：
    如果容器停止，请务必重启容器。如果手动停止，则仅在Docker守护程序重新启动或手动重新启动容器本身时才重新启动。（参见重启政策详情中列出的第二个项目）

unless-stopped：
    类似于always，除了当容器停止（手动或其他方式）时，即使在Docker守护程序重新启动后也不会重新启动容器。
```
