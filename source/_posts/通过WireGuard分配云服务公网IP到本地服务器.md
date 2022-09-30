---
title: 通过WireGuard分配云服务公网IP到本地服务器
date: 2022-09-28 12:25:37
tags: [WireGuard, 公网IP, 公网]
sidebar: [toc]
---

### 基础环境介绍

云服务器：腾讯云1h2g轻量云服务器
本地：DellR230机架式服务器（电信家用宽带）
均使用Linux系统（经测试不适用于windows系统）

### 实战步骤

#### 安装wireguard

首先我们要在两台机器间建立一个虚拟局域网（除了wireguard，zerotier之类的软件也是可以的），先在本地与远程两台机器上安装wireguard，并生成公钥和私钥。

```
# 安装wireguard
apt install wireguard
# 生成公钥和私钥
wg genkey  | tee privatekey | wg pubkey > publickey
```
#### 开启转发规则
【重点】公网服务器端开启端口转发规则，修改 /etc/sysctl.conf 添加 net.ipv4.ip_forward = 1 执行 sysctl -p 开启转发

#### 写入配置文件
然后在两台服务器/etc/wireguard/ 下创建配置文件 wg.conf
```
[Interface]
# 客户端连接的端口
ListenPort = 16000 
# 虚拟局域网中的ip
Address = 10.0.0.1/24  
# 填上之前生成的私钥
PrivateKey = $PRIVATEKEY   
# 下面两条是放行的iptables和MASQUERADE
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
# 无公网ip的服务器设置
[Peer]
# 无公网ip的服务器公钥
PublicKey = $PUBLICKEY 
AllowedIPs = 10.0.0.2/32
```
```
[Interface]
# 虚拟局域网中的ip
Address = 10.0.0.2/32
# 客户端私钥
PrivateKey = CLIENTA_PRIVATE_KEY
# MTU（可选）
# MTU = 1420
# 监听端口（可选）
# ListenPort = 

[Peer]
# Server
# Server 公钥
PublicKey = SERVER_PUBLICKEY
# Server IP:Port
Endpoint = 公网服务器IP:16000
# 通过 wireguard传输的 IP 列表
AllowedIPs = 10.0.0.0/24
# 强烈建议开启定期握手，不然远程服务器可能没办法主动连接上本地机器
PersistentKeepalive = 20
```
#### 启动wireguard
```
wg-quick up wg
# 或者 
systemctl start wg-quick@wg
```
### 疑难杂症
#### 虚拟网不通
按照前面的步骤写入的配置文件中，云服务器的wg虚拟地址为10.0.0.1，本地服务器的wg虚拟地址为10.0.0.2

如果从云服务器中使用ping命令验通10.0.0.2时存在问题，就应该先检查wg客户端于服务端之间的通信

常见问题如AllowedIPs =10.0.0.0/24，24代表的是相应的子网掩码，也就是说10.0.0.0/24代表了10.0.0.1~10.0.0.254中的所有地址，如果将24更改为32（即10.0.0.1/32）为仅指向10.0.0.1一个地址

这种情况下应当优先检查客户端和服务端的wg.conf配置文件
#### 外网不通
如果在确认配置文件中没有问题的情况下，外网依旧不通，优先检查iptables。通过iptables-save命令查看当前iptables规则，然后搭配tcpdump工具检查网卡流量情况来判断问题出现位置。

如果确定问题不出在配置文件中的情况时，可通过reboot重启服务器来实现清空iptables规则（剩下的交给玄学QAQ）

### 我使用的配置文件
```
[Interface]
# 虚拟局域网中的ip
Address = 10.0.0.2/24
# 客户端私钥
PrivateKey = 客户端私钥
Table = 1
PostUp = ip rule add from 10.0.0.2 table 1; ip rule add to 10.0.0.0/24 table 1
PostDown = ip rule del from 10.0.0.2 table 1; ip rule del to 10.0.0.0/24 table 1


[Peer]
# Server
# Server 公钥
PublicKey = 服务器公钥
# Server IP:Port
Endpoint = 服务器公网IP:16000
# 通过 wireguard传输的 IP 列表
AllowedIPs = 10.0.0.1/32, 0.0.0.0/0
# 强烈建议开启定期握手，不然远程服务器可能没办法主动连接上本地机器
PersistentKeepalive = 20
```
```
[Interface]
# 客户端连接的端口
ListenPort = 16000
Address = 10.0.0.1/24
# 填上之前生成的私钥
PrivateKey = 公网服务器私钥

PostUp = iptables -t nat -A PREROUTING -d 10.0.16.16 -p tcp -m multiport --dports 22,16000 -j ACCEPT; iptables -t nat -A PREROUTING -d 10.0.16.16 -p udp -m multiport --dports 22,16000 -j ACCEPT; iptables -t nat -A PREROUTING -d 10.0.16.16 -j DNAT --to-destination 10.0.0.2; iptables -t nat -A POSTROUTING -s 10.0.0.2 -j SNAT --to-source 10.0.16.16;
PostDown = iptables -t nat -D PREROUTING -d 10.0.16.16 -p tcp -m multiport --dports 22,16000 -j ACCEPT; iptables -t nat -D PREROUTING -d 10.0.16.16 -p udp -m multiport --dports 22,16000 -j ACCEPT; iptables -t nat -D PREROUTING -d 10.0.16.16 -j DNAT --to-destination 10.0.0.2; iptables -t nat -D POSTROUTING -s 10.0.0.2 -j SNAT --to-source 10.0.16.16;

# 无公网ip的服务器设置
[Peer]
# 无公网ip的服务器公钥
PublicKey = 本地服务器公钥
AllowedIPs = 10.0.0.2/32
```

### 参考
https://fuckcloudnative.io/posts/wireguard-docs-practice/#-dns
https://www.cnblogs.com/wanstack/p/7728785.html
https://www.liuvv.com/p/a8480986.html
https://aoyouer.com/posts/wireguard-l3-forward/


