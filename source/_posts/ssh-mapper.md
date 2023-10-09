---
title: 端口映射
cover: https://cdn.pixabay.com/photo/2023/09/05/12/29/boy-8235025_1280.jpg
categories: 
- 环境搭建
tags:
- git
date: 2023-08-08 18:30:43
---

当一台服务器拥有公网IP，或者域名解析的时候（这个可以使用ddns进行配置），我们其他的服务器上面的服务也想映射出来，那么我们可以选择重新获取一个公网IP或者域名解析，但是这种方法会增加我们的消费，下面我们使用ssh来实现端口映射，从而实现其他服务器上的服务也能实现外网访问的操作

<!--more-->

## 使用命令

转发到远端：ssh -C -f -N -g -L 本地端口:目标IP:目标端口 用户名@目标IP

转发到本地：ssh -C -f -N -g –R 本地端口:目标IP:目标端口 用户名@目标IP

```
-C，是进行数据压缩。

-f，是后台认证用户/密码，通常和-N连用，不用登录到远程主机。只有当提示用户名密码的时候才转向前台。

-N，是不执行远端命令，在只是端口转发时这条命令很有用处。

-g ，在-L/-R/-D参数中，是允许远端主机连接本地转发端口，如果不加这个参数，只允许本地主机建立连接。

-L，则是将本地端口映射到远端主机端口。本地端口:目标IP:目标端口

-R，表明是将远端主机端口映射到本地端口。本地端口:目标IP:目标端口
```