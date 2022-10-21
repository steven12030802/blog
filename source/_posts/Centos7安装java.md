---
title: Centos7安装java
sidebar:
  - toc
date: 2022-10-21 15:22:36
tags: [Linux,Java,Centos]
---
## 查看当前系统JAVA版本
```
java -version
```
![](https://img-blog.csdnimg.cn/img_convert/ba47d99815086bd59d01939d7e3e81da.png "url")

## 查看卸载库中已有的版本
```
rpm -qa | grep jdk
rpm -e --nodeps java-1.8.0-openjdk-1.8.0.262.b10-0.el7_8.x86_64
```
![](https://img-blog.csdnimg.cn/img_convert/2e2d1034c214c74df2a071fe9f7216b3.png "url")

## 安装指定版本
```
yum install java-11-openjdk* -y
```

## 切换JAVA版本
```
alternatives --config java
```
![](https://img-blog.csdnimg.cn/img_convert/5ec0fa036344e575e9f80e114e8a4843.png#pic_center "url")

## 其它
执行rpm命令过程中如果出现`cannot open Packages database in /var/lib/rpm`,可执行如下操作
```
cd /var/lib/rpm
ls
rm -rf __db*
rpm --rebuilddb
```