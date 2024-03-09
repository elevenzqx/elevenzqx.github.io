---
title: Git SSH 配置流程
cover: https://cdn.pixabay.com/photo/2023/09/05/12/29/boy-8235025_1280.jpg
categories: 
- 环境搭建
tags:
- git
date: 2024-02-02 18:30:43
---

当一台服务器拥有公网IP，或者域名解析的时候（这个可以使用ddns进行配置），我们其他的服务器上面的服务也想映射出来，那么我们可以选择重新获取一个公网IP或者域名解析，但是这种方法会增加我们的消费，下面我们使用ssh来实现端口映射，从而实现其他服务器上的服务也能实现外网访问的操作

<!--more-->

## 使用命令

### 检查 SSH 密钥是否存在

打开 Terminal 终端，输入 `cd ~/.ssh` 进入 ssh 目录。然后输入 `ls` 查看该目录下的文件。如果看到 `id_rsa` 和 `id_rsa.pub` 这两个文件，说明已经存在 SSH 密钥，可以直接使用。如果没有，则需要生成新的 SSH 密钥。

### 生成新的SSH密钥

在 Terminal 终端中，输入 

```
ssh-keygen -t rsa -C "your_email@example.com"
```

然后直接按 `Enter` 键。这里，`your_email@example.com` 应该替换为你的邮箱地址。
命令执行完毕后，会在 `~/.ssh` 目录下生成 `id_rsa`（私钥）和 `id_rsa.pub`（公钥）两个文件。

### 查看SSH公钥

在Terminal终端中，输入

```
cat ~/.ssh/id_rsa.pub
```

然后按下 `Enter` 键。这样就可以查看到你的 SSH 公钥内容。

### 添加SSH公钥到Git服务器

登录你的Git服务器账户（例如GitHub、GitLab等），找到SSH公钥的设置选项，将上一步查看到的SSH公钥内容粘贴进去，并保存。

```
Github地址: https://github.com/settings/keys
```

### 配置 Email 信息

在一些开源项目中，如果提交代码时，会要求有提交者的身份信息，这里需要为你的本地用户添加用户名与邮箱地址，故而有以下配置：

```
git config --global user.name "你的用户名"  
git config --global user.email "你的邮箱地址"
```
