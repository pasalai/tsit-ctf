---
title: HCIA学习笔记(5/5)
date: 2020-01-03 09:20:13
tags: [笔记,网络,HCIA,网工]
categories: [笔记 ]
---

# HCIA学习笔记(5/5)
## DHCP应用场景：
* DHCP服务器为网络内的设备自动分配IP地址
## DHCP报文：
![报文类型 含义](https://i.jpg.dog/img/967abba6cfa20c4c997b2c3500d13fb8.png)	
<!--more-->
## DHCP工作原理：
### 主机A	DHCP服务器
1. DHCP Discover(广播)→
2. DHCP Offer(单播)←
3. DHCP Request(广播)→
4. DHCP ACK(单播)←
> DHCP续约两个点50%和87.5%，如未收到服务器响应，则会申请重新绑定IP

## 服务器命令：
<pre>
DHCP enable //全局开启DHCP服务
ip pool <名称> //创建地址池及名称
network 192.168.1.0 mask 24 //定义地址池地址及掩码
gatway-list <网关地址>192.168.1.254 //定义网关
dns-list <DNS服务器地址> //定义DNS服务器
lease day 3 hour 3 minute 3 //修改租期
undo ip pool <名称> //删除整个地址池
</pre>
<pre>
dhcp select global //端口下下发IP地址选择全局IP地址
dhcp select interface //端口dhcp下发改为端口
dhcp server dns-list 8.8.8.8 //定义DNS服务器
</pre>

## NAT网络地址转换：
* 相关命令：
<pre>在路由器的wan口添加缺省静态路由：ip router-static 0.0.0.0 0 gigabitethrnet 0/0/0 202.102.1.1
端口下添加NAT静态转发规则：nat static global <外网地址> inside <内网地址>
</pre>
* 创建NAT地址池：
<pre>nat address-group <1-7> <起始IP> <结束IP>
如：nat address-group 1 202.102.1.100 202.102.1.150
</pre>
## ACL访问控制协议：
* 配置ACL协议命令：
<pre>
acl 2000 //acl参数2000-4999分为三段具有不同含义，详情参考命令说明
rule permit source 192.168.1.0 0.0.0.255 //此处的掩码为通配符掩码，0为严格匹配
</pre>
## ACL与NAT结合使用：
<pre>
nat outbound 2000 address-group 1 //将acl 2000的包通过nat group1转发出去
</pre>

## EasyIP配置：
<pre>nat outbound 2000 //网内主机少时，直接可以不用划分address-group，通过边界路由器的wan口地址直接转发</pre>

