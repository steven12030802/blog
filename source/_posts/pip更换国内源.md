---
title: pip更换国内源
sidebar:
  - toc
date: 2023-02-27 11:31:19
tags: [pip, 换源, 国内源]
---

# 镜像源：

清华：https://pypi.tuna.tsinghua.edu.cn/simple

阿里云：http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

华中科技大学：http://pypi.hustunique.com/

山东理工大学：http://pypi.sdutlinux.org/

豆瓣：http://pypi.douban.com/simple/

中科大：https://pypi.mirrors.ustc.edu.cn

# 换源
## 临时换源：
`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple package_name`
## 永久还原：
`pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple`
## 其它方法-手动添加配置文件：
在C:\Users\Administrator下创建一个pip文件夹，在里面创建pip.ini，并配置内容：
```
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/ 
trusted-host = pypi.tuna.tsinghua.edu.cn
```
