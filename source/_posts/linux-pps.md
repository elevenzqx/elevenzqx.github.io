---
title: Linux系统中网络PPS值测量
categories:
  - linux
tags:
  - linux
  - 网络
date: 2020-04-31 22:07:23
cover: https://cdn.pixabay.com/photo/2023/09/25/07/37/eagle-owl-8274405_1280.jpg
thumbnail: https://images.pexels.com/photos/17093201/pexels-photo-17093201.jpeg?auto=compress&cs=tinysrgb&w=1600&lazy=load
---

网络设备的转发性能以“包转发性能”来表示，即设备在单位时间内能够处理多少个“包”，这决定了设备转发能力的强弱。 
包转发性能比较常见的单位是“PPS”，即Packet Per Second(包每秒)。

<!--more-->

## 线速转发

如果一个端口（包括交换机、服务器）在满负载的情况下，对帧进行无差错的转发称为线速转发。

由以太网的设计规则，以太网帧最小64字节，根据端口速度，计算可以得出线速转发的PPS值。

![](https://www.ipcpu.com/wp-content/uploads/2017/12/wpid-1f08620fa640b0e842949e1d43077f56_0.9533148854970932.png)

![](https://www.ipcpu.com/wp-content/uploads/2017/12/wpid-1f08620fa640b0e842949e1d43077f56_0.39345882600173354.png)


## Linux下如何统计出PPS值

### 使用sar命令

```
sar -n DEV 5 10000
```

编写shell脚本可以获取PPS值

```
#!/bin/bash
INTERVAL="10"  # update interval in seconds
while true
do
        R1=`cat /sys/class/net/eth0/statistics/rx_packets`
        T1=`cat /sys/class/net/eth0/statistics/tx_packets`
        sleep $INTERVAL
        R2=`cat /sys/class/net/eth0/statistics/rx_packets`
        T2=`cat /sys/class/net/eth0/statistics/tx_packets`
        TXPPS=`expr $T2 - $T1`
        RXPPS=`expr $R2 - $R1`
    #
    TPS=`expr $TXPPS / 10`
    RPS=`expr $RXPPS / 10`
        echo "TX: $TPS pps  RX: $RPS pps"
        echo "TX: $TPS pps  RX: $RPS pps" >> /tmp/pps.log
done
```

## 如何测试网卡极限PPS值

网络的带宽是恒定的，为了获取更多的PPS，可以将数据包减到最小，由于TCP包需要建立连接而UDP不需要，因此UDP小包是测试PPS极限值的最佳选择。

根据CloudFlare（参考资料1）里面的提示，我们根据其源码，编译了udp发包和收包的程序，并将其运行在服务器上。

如下图，我们使用了3台虚拟机客户端发包，1台虚拟机收包，其结果如下：

![](https://www.ipcpu.com/wp-content/uploads/2017/12/wpid-1f08620fa640b0e842949e1d43077f56_0.4832962350254899.png)

10.140.1.22为接收端，其余为发送端，根据图中显示，发送数据量PPS接近26万，而接收端只能接受20万左右。

实际测试中，系统由于网络PPS达到了极限，SSH无法登陆，可以登陆控制台查看日志。

## 如何优化

一般来说虚拟机差不多能达到15W-20W的PPS，物理机是100W多，如果低于平均水平，需要检查和优化下相关内核的网络参数了。

## 参考资料

https://blog.cloudflare.com/how-to-receive-a-million-packets/ 
http://www.jianshu.com/p/242e9b930240 
https://kb.juniper.net/InfoCenter/index?page=content&id=KB14737

