---
title: debian11安装docker
sidebar:
  - toc
date: 2023-02-22 15:12:17
tags:
---

## 安装基础工具
```
sudo apt-get update
 sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

## 安装docker的gpg key
```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

## 安装docker源
```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
上面命令中的lsb_release -cs返回bullseye，也就是debian11的代号

## 安装docker
```
apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
至此安装完成。
