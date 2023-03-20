---
title: centos7安装sshd
sidebar:
  - toc
date: 2023-03-20 10:59:01
tags:
---
1. 安装sshd服务
```
yum install -y openssl openssh-server

yum install openssh*
```
2. 启动sshd服务

```
systemctl enable sshd

systemctl start sshd

service sshd start
```
3. 添加防火墙规则
```
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```