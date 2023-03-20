---
title: CentOS7安装SQLServer数据库服务
sidebar:
  - toc
date: 2023-03-20 10:54:35
tags:
---
1. 下载 Microsoft SQL Server 2019 Red Hat 存储库配置文件
```
sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
```
2. 安装 SQL Server
```
sudo yum install -y mssql-server
```
3. 安装完成后，运行 mssql-conf setup，按照提示设置 SA 密码并选择版本\
```
sudo /opt/mssql/bin/mssql-conf setup
```
4. 完成配置后，验证服务是否正在运行
```
systemctl status mssql-server
```
5. 若要允许远程连接，请在 RHEL 的防火墙上打开 SQL Server 端口。 默认的 SQL Server 端口为 TCP 1433。 如果为防火墙使用的是 FirewallD，则可以使用以下命令
```
sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
sudo firewall-cmd --reload
```
此时，SQL Server 2019 正在 RHEL 计算机上运行，随时可以使用！


安装 SQL Server 命令行工具
1. 下载 Microsoft Red Hat 存储库配置文件
```
sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
```
2. 如果安装了早期版本的 mssql-tools，请删除所有旧的 unixODBC 包
```
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel
```
3. 运行以下命令，以使用 unixODBC 开发人员包安装 mssql-tools
```
sudo yum install -y mssql-tools unixODBC-devel
```
4. 向 PATH 环境变量添加 /opt/mssql-tools/bin/。 这样可以在不指定完整路径的情况下运行这些工具。 运行以下命令以修改登录会话和交互式/非登录会话的路径：
```
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
```



