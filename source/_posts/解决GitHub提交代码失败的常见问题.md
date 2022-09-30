---
title: 解决GitHub提交代码失败的常见问题
sidebar:
  - toc
date: 2022-09-28 13:29:27
tags: [git, ssh, key]
---
# 解决Please make sure you have the correct access rights and the repository exists 问题.
## 问题:
```Please make sure you have the correct access rights and the repository exists ```
请确保您具有正确的访问权限并且存储库存在
## 原因:
公钥出问题了,需要删除.ssh下文件,然后重设置用户名和邮箱再重新生成ssh公钥即可解决
## 解决:
### 步骤一:
#### 删除.ssh下所有所有文件
路径: C:\Users\用户名\.ssh
### 步骤二:
#### 设置用户名
git config --global user.name ‘zhandehuang’

#### 设置用户名邮箱
git config --global user.email ‘it_zdh@163.com’

#### 查看设置
git config --list

然后继续输入命令:修改后面的邮箱即可
ssh-keygen -t rsa -C "it_zdh@163.com"
一路回车即可
### 步骤三
#### 配置:github
![这里输入图片描述](https://img-blog.csdnimg.cn/20200812211848812.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzA1MTMx,size_16,color_FFFFFF,t_70)
#### 配置SSH
![](https://img-blog.csdnimg.cn/20200812212021985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzA1MTMx,size_16,color_FFFFFF,t_70)
### 完成
![](https://img-blog.csdnimg.cn/20200812212258267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNzA1MTMx,size_16,color_FFFFFF,t_70)