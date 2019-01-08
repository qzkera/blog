---
title: H3Cv7--BGP联盟基本配置
date: 2019-01-08 21:24:11
tags:
 - BGP
 - H3C
 - 路由交换
---

#### 1. 联盟是处理自治系统内部的IBGP网络连接激增的一种方法，联盟将一个自治系统划分为若干个自治系统，每个自治系统内部的IBGP对等体建立全连接关系，子自治系统之间建立联盟内部EBGP连接关系。
#### 2.根据拓扑图指定子自治系统号，配置联盟ID指定一个联盟体中包含了哪些子自治系统。
#### 3.实验拓扑

![实验拓扑图](/images/bgp_lm01.jpg)

<!--more-->

#### 4.具体配置命令
##### RTA的主要配置

[RTA]bgp 65001
 <font color=red>[RTA-bgp-default]confederation id 200</font>
 <font color=red> [RTA-bgp-default]confederation peer-as 65002</font>
[RTA-bgp-default]peer 2.2.2.2 as-number 65001
[RTA-bgp-default]peer 2.2.2.2 connect-interface LoopBack 0
[RTA-bgp-default]peer 10.10.10.2 as-number 65002
[RTA-bgp-default]peer 100.1.1.1 as-number 100
[RTA-bgp-default]address-family ipv4
[RTA-bgp-default-ipv4]peer 2.2.2.2 en
[RTA-bgp-default-ipv4]peer 10.10.10.2 en
[RTA-bgp-default-ipv4]peer 100.1.1.1 en
#

##### RTB的主要配置

[RTB]bgp 65001
 <font color=red>[RTB-bgp-default]confederation id 200</font>
[RTB-bgp-default]peer 1.1.1.1 as-number 65001
[RTB-bgp-default]peer 1.1.1.1 connect-interface LoopBack 0
[RTB-bgp-default]add ipv4
[RTB-bgp-default-ipv4]peer 1.1.1.1 enable
#

##### RTC的主要配置

[RTC]bgp 65002
 <font color=red>[RTC-bgp-default]confederation id 200
 [RTC-bgp-default]confederation peer-as 65001</font>
[RTC-bgp-default]peer 4.4.4.4 as-number 65002
[RTC-bgp-default]peer 4.4.4.4 CON LO0
[RTC-bgp-default]peer 10.10.10.1 as-number 65001
[RTC-bgp-default]ADD IPV4
[RTC-bgp-default-ipv4]peer 10.10.10.1 EN
[RTC-bgp-default-ipv4]peer 4.4.4.4 enable
#
##### RTD的主要配置

[RTD]bgp 65002
 <font color=red>[RTD-bgp-default]confederation id 200</font>
[RTD-bgp-default]peer 3.3.3.3 as-number 65002
[RTD-bgp-default]peer 3.3.3.3 con lo0
[RTD-bgp-default]add ipv4
[RTD-bgp-default-ipv4]peer 3.3.3.3 enable
#

##### RTE的主要配置

[RTE]bgp 100
[RTE-bgp-default]peer 100.1.1.2 as-number 200
[RTE-bgp-default]add ipv4
[RTE-bgp-default-ipv4]peer 100.1.1.2 enable

#### 5.建立bgp对等体关系前AS200内的网络需要用IGP路由协议互通，建立完bgp邻居关系后公布网络，使用dis bgp routing-table查询bgp表…
