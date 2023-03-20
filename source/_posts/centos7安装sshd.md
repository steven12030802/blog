---
title: centos7安装sshd
sidebar:
  - toc
date: 2023-03-20 10:59:01
tags:
---

```
yum install -y openssl openssh-server

yum install openssh*

systemctl enable sshd

systemctl start sshd

service sshd start

sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
```