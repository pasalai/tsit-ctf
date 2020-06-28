---
title: HCIA学习笔记(3/5)
date: 2019/12/30 17:08:50 
tags: [笔记,网络,HCIA,华为,网工]
categories: [笔记 ]
---


# HCIA学习笔记（3/7）
![ping命令执行过程图示](https://i.jpg.dog/img/ba71c403eda7783dd6263612c26e5b2c.png)
# 路由器基本原理
## 路由器连接不同的LAN，LAN也称广播域进行数据转发。
##工作原理
* 解封装
* 查表
* 重封装
## 自治系统AS：
由一个管理机构管理、使用统一路由策略的路由器的集合。
## IP路由表： 

<pre>display IP routing-table</pre>
<!--more-->
### 路由表结构
![路由表结构](https://i.jpg.dog/img/911fd30b253e3efc022400cfafb0db64.png)
### 路由优先级pre
![路由优先级pre](https://i.jpg.dog/img/094c796dab6dc0cb8214061e8ff912f8.png)
### 路由选路：
1. 最长匹配原则：两条相同路由，但子网掩码不同时，选掩码最长的
2. 路由优先级pre：小优先原则
3. 路由度量cost：小优先原则
4. 等价路由随机选择路径

### 路由器转发数据：
目的网段/掩码+出接口+下一跳

# 三层技术：静态路由协议的作用及基本配置、浮动静态路由、缺省路由
![学习示例拓扑](https://i.jpg.dog/img/46aef3877887c95e1a882047c156fc2b.png)
## 添加静态路由，命令格式：`ip route-static 目标地址 掩码 出端口 下一跳`
<pre>
R1路由器:ip route-static 192.168.2.1 24 gigabitethernet 0/0/1 192.168.2.2
R2路由器:ip route-static 192.168.1.1 24 gigabitethernet 0/0/1 192.168.2.1
</pre>
## 浮动静态路由：
* 两条链路，写入两条静态路由，并更改其中一条路由的优先级，产生容灾性，静态路由备份。
* 当优先级高的路由故障时，优先级低的路由浮现出来进行容灾，称为浮动路由。

## 路由器的环回路由端口：
* loopback0/1/2/.......

## 缺省路由：
* 一条缺省路由可以代替多条明细路由。
* <pre>示例命令：ip route-static 0.0.0.0 0 GigabitEthernet 0/0/0 172.16.1.2</pre>

# 三层技术：动态路由协议介绍
## 动态路由——动态路由协议：
* RIP:趋于淘汰
* OSPF：范围相对较小，辖区内
* ISIS：范围相对中等，市间
* BGP：范围相对较大,省间

## 路由的分类:IGP与EGP、数据链路状态与距离矢量
### 部署范围：
* AS内（IGP内部网关协议）：RIP、OSPF、ISIS
* AS外（EGP外部网关协议）：BGP
### 工作方式：
* 距离矢量 RIP BGP
* 链路状态 OSPF ISIS
* 区别：距离矢量易出现环路
## RIP 路由信息协议：
是基于距离矢量算法的协议，一般适用于小型网络中。配置简单，易于维护。
### RIP工作原理：
* 路由器运行RIP后，会首先发送路由更新请求，收到请求的路由器会发送自己的RIP路由进行响应。
* 网络稳定后，路由器会周期性的发送路由更新信息。
### RIP-度量-开销（cost）:
* RIP使用跳数作为度量值来衡量到达目的网络的距离；
* 缺省情况下，直连网络的路由跳数为0。当路由器发送路由更新时，会把度量值加1 ， RIP规定超过15跳为网络不可达。
### RIPv1包结构
![RIPv1包结构](https://i.jpg.dog/img/b40a527abf505d58494f9715cb5a7574.png)
### RIPv2包结构
![RIPv2包结构](https://i.jpg.dog/img/5d9b50417827a82bd769505ffac84488.png)
### RIPv1与v2的区别
![RIPv1与v2的区别](https://i.jpg.dog/img/0309bb9b799ce0ef163f3a46302130fb.png)
### RIPv2认证
![RIPv2认证](https://i.jpg.dog/img/94312574af005cc3e749812ac1645c74.png)
### 相关命令
<pre>
rip <进程号>
rip进程中，version指令可以查看当前的RIP版本号，version2或version1切换版本
rip进程中，network指令可以声明主类网络，如：192.168.1.0或10.0.0.0或172.1.0.0
RIPv2协议中，在端口下时，可以用rip authentication-mode md5 usual huawei来指定MD5认证方式传输路由表
</pre>
## RIP环路：
rip两节点的抛出时间不同，网内环境变化，可能会导致环路，但这种环路是暂时的。
### 防环机制：
* 水平分割：rip split-horizon
* 毒性反转：端口接到路由后，将cost直接置为16后发回，使其失效防环（rip poison-reverse）
* 触发更新：网内环境变化时，立即向其他路由发送更新
* 最大跳数：16跳时丢弃该条路由
* cost操作：<pre>
rip metricout 2:更改端口发出路由时的COST为+2
rip metricin 2:更改端口接收到路由时的COST为+2
</pre>