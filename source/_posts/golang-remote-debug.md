---
title: Golang进行远程调试
categories:
  - go
tags:
  - go
date: 2020-05-25 20:18:13
cover: https://images.pexels.com/photos/17093201/pexels-photo-17093201.jpeg?auto=compress&cs=tinysrgb&w=1600&lazy=load
thumbnail: https://images.pexels.com/photos/17093201/pexels-photo-17093201.jpeg?auto=compress&cs=tinysrgb&w=1600&lazy=load
---

> 远程调试golang代码需要在运行代码的远程机器上按照delve,然后以delve运行要调试的程序

* 编译

```shell
export CGO_ENABLED=0 GOOS=linux GOARCH=amd64
go build -gcflags='all -N -l' main.go
```

<!--more-->

* install delve

```shell
go get -u github.com/derekparker/delve/cmd/dlv
```

* delve 运行程序

```shell
dlv --listen=:2345 --headless=true --api-version=2 exec ./main
```

* goland 设置remote debug
  * host为远程主机ip 端口是刚才dlv设置的端口

* debug
  * 然后就像调试本地代码一样调试远程主机上的程序
