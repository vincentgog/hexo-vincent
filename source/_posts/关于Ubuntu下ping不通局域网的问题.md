---
title: 关于Ubuntu下ping不通局域网的问题
date: 2021-12-24 19:25:00
author: Vincent
toc: true
summary: linux出现连不通内网的情况说明和解决办法
categories: 技术
tags:
  - Linux
  - Debug
---

## 问题出现背景
使用synergy软件，可以用一套键鼠控制不同的两台电脑（win+linux），但是前提是两台电脑需要连接到同一局域网中。

具体情况信息：

> 无线连接 
> Linux的IP地址：192.168.1.148 
> Win的IP地址：192.168.1.110 
> Linux和Win的防火墙均关闭

在具体的操作过程中，出现了很奇怪的现象：在Ubuntu下，在终端`Ping 192.168.1.110`会出现 `From 192.168.1.1 icmp_seq=1 Destination Host Unreachable`，即在Linux下连不通内网，但是上外网是可以的。
当然，Linux下`Ping 192.168.1.148`（自己）是正常的。

## 问题分析及解决
在终端下输入`arp -a`，会出现

    ?(192.168.1.110) 位于 <incomplete> 在 eth1
    ?(192.168.1.148) 位于<incomplete> 在 eth1
    ?(192.168.1.1) 位于 b0:---0d [ether] 在 wlan1

虽然不知道具体表达什么意思，似乎内网其他ip地址不是在合适的地方，网卡eth1出来捣乱了。所以我把eth1禁用试了一下：`sudo ifconfig eth1 down`，再执行`arp -a`会显示

    ?(192.168.1.110) 位于 40:--:7b [ether] 在 wlan1
    ?(192.168.1.1) 位于 b0:---0d [ether] 在 wlan1

结果再执行`ping 192.168.1.110`就可以ping通了。

## 总结
解决Ping不通的问题，需要考虑的方向：

> 1. 防火墙问题，是否关闭了 
> 2. 网卡问题，是否别的网卡影响无线网卡 
> 3. 局域网问题，确保路由器正常或两台电脑确定连在同一路由器上，确保网段一致