---
title: H3Cv7—BGP反射基本配置
date: 2019-01-11 09:46:34
tags:
 - BGP
 - H3C
 - 实验
---

#### 1.	路由反射可替代IBGP对等体全连接，路由反射原理就是允许某些网络设备将从IBGP对等体处接受到的路由信息发布给其他特定的IBGP对等体；
#### 2.	根据实验拓扑配置路由反射器及客户机非客户机。
#### 3. 实验拓扑
![实验拓扑图](/images/bgpFS.jpg)

<!--more-->

#### 4.各设备的主要配置
- RTA的重点配置

```
[RTA]bgp 200
[RTA-bgp-default]group qzk internal 
[RTA-bgp-default]peer qzk connect-interface LoopBack 0
[RTA-bgp-default]peer 2.2.2.2 group qzk
[RTA-bgp-default]peer 3.3.3.3 group qzk
[RTA-bgp-default]peer 4.4.4.4 as-number 200
[RTA-bgp-default]peer 4.4.4.4 connect-interface LoopBack 0
[RTA-bgp-default]peer 192.168.1.2 as-number 100
[RTA-bgp-default]address-family ipv4 unicast 
[RTA-bgp-default-ipv4]peer qzk enable 
[RTA-bgp-default-ipv4]peer qzk reflect-client 
[RTA-bgp-default-ipv4]peer 4.4.4.4 enable 
[RTA-bgp-default-ipv4]peer 192.168.1.2 enable

```

- RTB的重点配置

```
[RTB]bgp 200
[RTB-bgp-default]peer 1.1.1.1 as-number 200
[RTB-bgp-default]peer 1.1.1.1 connect-interface LoopBack 0
[RTB-bgp-default]address-family ipv4
[RTB-bgp-default-ipv4]peer 1.1.1.1 enable

```

- RTC的重点配置

```
[RTC]bgp 200
[RTC-bgp-default]peer 1.1.1.1 as-number 200
[RTC-bgp-default]peer 1.1.1.1 connect-interface LoopBack 0
[RTC-bgp-default]address-family ipv4
[RTC-bgp-default-ipv4]peer 1.1.1.1 enable

```
- RTD的重点配置

```
[RTD]bgp 200
[RTD-bgp-default]peer 1.1.1.1 as-number 200
[RTD-bgp-default]peer 1.1.1.1 connect-interface LoopBack 0
[RTD-bgp-default]address-family ipv4
[RTD-bgp-default-ipv4]peer 1.1.1.1 enable

```
- RTE的基本配置

```
[RTE]bgp 100
[RTE-bgp-default]peer 192.168.1.1 as-number 200
[RTE-bgp-default]address-family ipv4 unicast 
[RTE-bgp-default-ipv4]peer 192.168.1.1 enable

```

#### 5. 定义
<font face="微软雅黑" color=black>（1）如果路由更新是从非客户端收到的，仅反射给客户机
（2）如果路由更新是从客户机收到的，反射给所有非客户机及客户机，除了这个路由器更新的始发者
（3）如果路由更新是从EBGP对等体收到的，反射所有的客户机和非客户机</fout>

<font face="微软雅黑" color=green size=3> 如要证明这三点，则在该实验基础上再加一个非客户机，并在每台路由器上公布自己的环回口地址，使用dis bgp routing-table来查询每台路由器上得bgp表验证该三条定义 </font>
