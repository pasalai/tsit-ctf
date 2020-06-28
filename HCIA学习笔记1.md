---
title: HCIA学习笔记（1/5）
date: 2019-12-23 17:31:36
tags: [笔记,网络]
categories: [笔记 ]
---

# OSI开放式互联参考模型：    
![2ece8f17c1c9d07ff007629a6d3e7674.png](https://i.jpg.dog/img/2ece8f17c1c9d07ff007629a6d3e7674.png)
<!--more-->
# TCP/IP参考模型与OSI对应：    
![a145a790f2640c3152c6b53104e8b726.png](https://i.jpg.dog/img/a145a790f2640c3152c6b53104e8b726.png)

# 分析：
[![5cf5c6f8c71465dccc946ae21aa2b4cf.png](https://i.jpg.dog/img/5cf5c6f8c71465dccc946ae21aa2b4cf.png)](https://jpg.dog/i/O7GKW)

# 封装：
传输层→网络层→数据层→物理层		的数据加工过程
# 解封装
# ISP互联网络提供商
# 传输层定义的两种协议：
* TCP(传输控制协议)：面向连接的传输层协议，提供可靠的传输服务；
* UDP(用户数据包协议)：
# TCP端口号：
![382f83ab0cb69dea211dc627e7f4bcc0.png](https://i.jpg.dog/img/382f83ab0cb69dea211dc627e7f4bcc0.png)
# TCP头部(16字节)：
[![18b9a30f73683b0d2b6f7a03cd132b76.png](https://i.jpg.dog/img/18b9a30f73683b0d2b6f7a03cd132b76.png)](https://jpg.dog/i/O7qOK)
[![5e76bd78c246d907dbc891650f19992b.png](https://i.jpg.dog/img/5e76bd78c246d907dbc891650f19992b.png)](https://jpg.dog/i/O7VJt)

# 端口号分类：
* 知名端口号：1-1023
* 注册端口号：1024-49151
* 随机端口号：49152-65535
# UDP端口号(8字节)：
![668d2656f59ac1dc138fa70012c2fdd6.png](https://i.jpg.dog/img/668d2656f59ac1dc138fa70012c2fdd6.png)

# UDP头结构：
![1537f6943f35737291485e85ed483e55.png](https://i.jpg.dog/img/1537f6943f35737291485e85ed483e55.png)
# TCP传输过程：
## 三次握手
* 主机A→服务器A：Send SYN(seq=a,SYN)
* 服务器A→主机A：send SYN,ACK(seq=b,ack=a+1,SYN,ACK)
* 主机A→服务器A：send ACK(seq=a+1,ack=b+1,ACK)
## 流量控制
## 丢包重传:
* 丢包时接收方检测到后会重新发送该段的确认号，待确认无误，才会发送下一个段的确认号
## 四次挥手
[![c31e1095c6142f3964d0ef0373204d5b.png](https://i.jpg.dog/img/c31e1095c6142f3964d0ef0373204d5b.png)](https://jpg.dog/i/O7HfN)


# 路由器系统视图：
[![4c50ea8de8362d62d3be5f1046230e38.png](https://i.jpg.dog/img/4c50ea8de8362d62d3be5f1046230e38.png)](https://jpg.dog/i/O7USk)

# 常用命令：
```
查看所有接口的IP地址及状态：display ip interface brief
查看当前端口的配置：display this
为当前端口配置IP地址：ip address 地址 掩码位数
当前端口初始化：undo
保存配置：save 仅在用户视图下有效
推回上一层：quit/q
```
