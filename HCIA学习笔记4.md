---
title: HCIA学习笔记(4/5)
date: 2019/12/31 07:08:50 
tags: [笔记,网络,HCIA,华为,网工]
categories: [笔记 ]
---

# HCIA学习笔记(4/5)

# OSPF-链路状态路由协议：开放式最短路径优先(OSPF)协议
## 版本分为
* v2适用于IPv4
* v3适用于IPv6
## 链路状态信息LSA(Link State)
* 链路类型
* 链路开销
* 链路接口IP
* 链路接口子网掩码
* 链路上的路由器
## 链路状态数据库（LSDB）
* 保存链路状态信息
<!--more-->
## LSA泛洪
* 同步了链路数据库LSDB,同步了路由表
## LSA协议工作方式
* LSA泛洪
* 同步LSA
* 计算LSA
## OSPF报文封装在IP报文中，协议号为89
##OSPF报文类型
* Hello报文：发现ospf邻居，协商OSPF邻居，建立OSPF邻居，维护OSPF邻居(周期性的发包)
* DD(database description)数据库描述报文：互相描述数据库
* LSR(LSA Request)报文:用于请求同步
* LSU(LSA Updata):更新，用于同步数据库
* LSACK(LSA Acknowledgement)报文:用于确认数据库同步
## OSPF邻居状态机
* 作用：标识OSPF工作在那个阶段
* ![OSPF邻居状态机](https://i.jpg.dog/img/c7d7e46c38d6107b700910f58645444c.png)

## Route ID、邻居和邻接
routeID格式类似于IP地址，若不指定，则OSPF自动选择一个接口IP地址作为routeID
## OSPF区域
![OSPF区域](https://i.jpg.dog/img/7ce4365a43c9a29295f684f7ff41e2a8.png)
* 每个区域都维护一个独立的LSDB
* Area 0 是骨干区域，其他区域都必须于此区域相连
* 区域是基于路由器接口划分的
* RTA、RTB、RTC这种位于区域边界的路由器也被叫做区域边界路由器（ABR）
* AS边界路由器叫做ASBR，用BGP协议连接路由

## 划分OSPF区域的目的
* 减小LSDB
* 减轻路由器的压力
## DR邻接关系路由与BDR备份邻接关系路由选择条件
* 优先级小的优先，默认全为1
* 优先级相同时，路由ID越大的优先
## 为什么选择DR与BDR
* 减少LSA泛洪
* 减少邻接关系
## 邻居关系与邻接关系的区别
* 是否直接同步数据库
* 选举DR/BDR是基于路由器接口来选的
* DR有不可抢占性。
## OSPF路由协议相关命令
<pre>
ospf route-id 1.1.1.1
area 0
network 10.1.1.0 0.0.0.255
network 172.16.1.0 0.0.0.255
network 192.168.1.0 0.0.0.255
display ospf pear brief //查看所有OSPF邻居关系
display ospf interface gigabitethernet 0/0/0 //查看是否为DR
在某端口下：ospf dr-priority 100 //更改路由器端口选举DR的优先级（注意，DR确认后有不可抢夺性）
</pre>

# STP协议（生成树协议）
* RSTP:快速生成树协议
* MSTP:多生成树协议,默认运行的协议
## STP协议数据包
* BPDU桥协议数据单元
## 交换机环路引起的问题
* 广播风暴：交换机环路。
* MAC地址表震荡
## STP作用：
* 通过阻塞交换机的冗余端口达到防环的目的，并且实现链路备份
## 生成树的根被叫做根交换机，也叫根桥。
## root port (根端口选举)RP
* 非根交换机连接根交换机最优的端口
* 一台非根交换机只有一个非根端口
## Desidnate-port（指定端口）DP
* 交换机与交换机间链路到达根交换机最优的接口。
## Alternate-port堵塞端口（预备端口）AP
* 既不是根端口也不是指定端口，则是预备端口，将被阻塞，不进行通信。

## STP工作原理
1. 选举根交换机（根桥）
2. 每个非根桥交换机选举一个根端口
3. 每个网段选举一个指定端口

## 根桥选举规则
* 优先级越小越优先，默认32768，可以修改，只能改成4096的倍数，且范围在0-65535之间，BID桥ID
* 交换机MAC地址，越小越优先
## 根端口RD选举规则
* 根据路径开销越小越优先：百兆20W   千兆2W	
* 比较对端的桥ID，越小越优先
* 比较对端的PID,越小越优先(PID：端口优先级+端口号,优先级默认128,范围0-240,步长16）
## 指定端口DP选举规则
* 根路径开销，越小越优先
* 比较两端桥ID，越小越优先
* 比较本端的PID，越小越优先
## 交换机端口状态30秒构建生成树
* listening	侦听	15秒
* learning 学习	15秒
* 根端口	forwarding	转发
* 指定端口	forwarding	
* 预备端口	discard ing	关闭

# LAN局域网广播域
## VLAN(VLAN划分是基于交换机的接口的)
* 逻辑VLAN
* 虚拟VLAN
## 查看VLAN命令
<pre>
display vlan
display port vlan 
</pre>
## 创建VLAN命令
<pre>
vlan batch 10 20 30 //批量创建VLAN
vlan 40 //创建vlan并进入该vlan设置
</pre>
## VLAN三种链路类型
* Hybrid:华为独有，接入链接通用
* access:接入PC时的端口
* trunk:交换机之间的链接
## 划分端口到VLAN命令
* access：<pre>
port link-type access
port defult vlan 10
</pre>
* trunk:<pre>
port link-type trunk
port trunk pvid vlan 10
</pre>
* hybrid:<pre>
port link-type
port hybrid pvid vlan 10
</pre>

## 各链路类型对Tag处理
![各链路类型对Tag处理](https://i.jpg.dog/img/971fe11d497a9d1c0bb4cc1c334d59b0.png)
## 允许数据包通过交换机间的链路
<pre>port trunk allow-pass vlan 10 20</pre> 

## Trunk协议时数据包加tag及去tag标记命令
<pre>
port hybried tagged vlan 10 20 //加tag
port hybried tagged vlan 10 20 //去tag
</pre>