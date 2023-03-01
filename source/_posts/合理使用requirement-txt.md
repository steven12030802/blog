---
title: 合理使用requirement.txt
sidebar:
  - toc
date: 2023-03-01 11:04:12
tags:
---

# 使用requirement.txt标记所有需要安装的pip包
## 操作步骤
### 用`pip freeze > requirements.txt`自动生成requirement.txt

### 使用requirement.txt安装所有需要的包
安装requirement.txt，执行命令即可一键安装完所需要的第三方库。
命令：`pip install -r requirements.txt`

或者是：
第一步：用`activate your_environment`激活所配置的环境
第二步： `conda env export > environment.yaml` 来保存环境的yaml文件
第三步： 当我们想再次创建该环境，可以根据别人提供的.yaml文件复现环境，使用命令`conda env create -f environment.yaml`即可完成环境配置。
