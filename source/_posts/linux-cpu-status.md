---
title: linux CPU 状态信息
categories:
  - linux
tags:
  - linux
date: 2020-04-02 21:20:31
cover:
thumbnail:
---

linux 下 top 命令

<!--more-->

```shell
[readonly@vm-1584349070 server1]$ top
top - 12:04:34 up 16 days, 18:51,  1 user,  load average: 0.00, 0.03, 0.13
Tasks: 155 total,   1 running, 154 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.2 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem : 16267180 total, 13167984 free,  1579344 used,  1519852 buff/cache
KiB Swap:        0 total,        0 free,        0 used. 14287568 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                                                   
12534 device-+  20   0   13.6g 939824  15140 S   0.7  5.8   4:56.30 java                                                                                                                      
 1629 root      20   0       0      0      0 S   0.3  0.0   7:35.83 xfsaild/vda1                                                                                                              
 4597 readonly  20   0  162024   2308   1584 R   0.3  0.0   0:00.01 top                                                                                                                       
14323 readonly  20   0 4416872 225500   6436 S   0.3  1.4  10:28.40 media-server 
```

含义：

```shell
I try to explain  these:
us: is meaning of "user CPU time"
sy: is meaning of "system CPU time"
ni: is meaning of" nice CPU time"
id: is meaning of "idle"
wa: is meaning of "iowait" 
hi：is meaning of "hardware irq"
si : is meaning of "software irq"
st : is meaning of "steal time"
```

中文翻译为：

```markdown
us 用户空间占用CPU百分比
sy 内核空间占用CPU百分比
ni 用户进程空间内改变过优先级的进程占用CPU百分比
id 空闲CPU百分比
wa 等待输入输出的CPU时间百分比
hi 硬件中断
si 软件中断 
st: 实时
```
