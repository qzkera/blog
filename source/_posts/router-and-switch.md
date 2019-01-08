---
title: BGP 的 preferred-value 属性
date: 2018-11-08 11:59:24
tags: 
 - 路由交换
 - H3C
 - BGP
---

### 一.	实验要求：

修改preferred-value属性，使拓扑中的R1去往4.4.4.4网络的路径是从下面经过R3走的。

### 二.	实验拓扑：

![实验拓扑图](/images/router_1_01.jpg)

<!--more-->

### 三.	基本配置：

 - **R1**

```
[R1]int g0/0
[R1-GigabitEthernet0/0]ip add  12.1.1.1 24
[R1-GigabitEthernet0/0]int g0/1
[R1-GigabitEthernet0/1]ip add 13.1.1.1 24
[R1-GigabitEthernet0/1]int l0
[R1-LoopBack0]ip add 1.1.1.1 32
[R1-LoopBack0]int l1
[R1-LoopBack1]ip add 10.10.10.10 32
[R1-LoopBack1]qu
[R1]bgp 100
[R1-bgp-default]router-id 1.1.1.1
[R1-bgp-default]peer 12.1.1.2 as-number 200
[R1-bgp-default]peer 13.1.1.3 as-number 200
[R1-bgp-default]address-family ipv4
[R1-bgp-default-ipv4]peer 12.1.1.2 enable 
[R1-bgp-default-ipv4]peer 13.1.1.3 enable
[R1-bgp-default-ipv4]network 1.1.1.1 32
[R1-bgp-default-ipv4]network 10.10.10.10 32
[R1-bgp-default-ipv4]network 13.1.1.0 24
[R1-bgp-default-ipv4]network 12.1.1.0 24
```

</br>

 - **R2**

```
[R2]int g0/0
[R2-GigabitEthernet0/0]ip add 12.1.1.2 24
[R2-GigabitEthernet0/0]int g0/1
[R2-GigabitEthernet0/1]ip add 24.1.1.1 24
[R2-GigabitEthernet0/1]int l0
[R2-LoopBack0]ip add 2.2.2.2 32
[R2-LoopBack0]
[R2-LoopBack0]int g0/1
[R2-GigabitEthernet0/1]ip add 24.1.1.2 24
[R2-GigabitEthernet0/1]qu
[R2]ospf
[R2-ospf-1]area 0
[R2-ospf-1-area-0.0.0.0]network 2.2.2.2 0.0.0.0
[R2-ospf-1-area-0.0.0.0]network  24.1.1.0 0.0.0.255
[R2]bgp 200
[R2-bgp-default]router-id 2.2.2.2
[R2-bgp-default]peer 12.1.1.1 as-number 100
[R2-bgp-default]peer 4.4.4.4 as-number 200
[R2-bgp-default]peer 4.4.4.4 connect-interfac LoopBack0
[R2-bgp-default]add ipv4
[R2-bgp-default-ipv4]peer 4.4.4.4 enable 
[R2-bgp-default-ipv4]peer 12.1.1.1 enable
[R2-bgp-default-ipv4]network 2.2.2.2 32
[R2-bgp-default-ipv4]network 12.1.1.0 24
```

</br>

 - **R3**

```
[R3]int g0/0
[R3-GigabitEthernet0/0]ip add 13.1.1.3 24
[R3-GigabitEthernet0/0]int g0/1
[R3-GigabitEthernet0/1]ip add 34.1.1.3 24
[R3-GigabitEthernet0/1]int l0
[R3-LoopBack0]ip add 3.3.3.3 32
[R3-LoopBack0]qu
[R3]ospf 
[R3-ospf-1]area 0
[R3-ospf-1-area-0.0.0.0]network 34.1.1.0 0.0.0.255
[R3-ospf-1-area-0.0.0.0]network 3.3.3.3 0.0.0.0
[R3]bgp 200
[R3-bgp-default]router-id 3.3.3.3
[R3-bgp-default]peer 13.1.1.1 as-number 100
 [R3-bgp-default]peer 4.4.4.4 as-number 200
 [R3-bgp-default]peer 4.4.4.4 connect-interfac LoopBack0
 [R3-bgp-default]add ipv4
 [R3-bgp-default-ipv4]peer 4.4.4.4 enable 
 [R3-bgp-default-ipv4]peer 13.1.1.1 enable
 [R3-bgp-default-ipv4]network  3.3.3.3 32
 [R3-bgp-default-ipv4]network 13.1.1.0 24
```

</br>

 - **R4**

```
[R4]int g0/1
[R4-GigabitEthernet0/1]ip add 34.1.1.4 24
[R4-GigabitEthernet0/1]int g0/0
[R4-GigabitEthernet0/0]ip add 24.1.1.4 24
[R4-GigabitEthernet0/0]int l0
[R4-LoopBack0]ip add 4.4.4.4 32
[R4]ospf
[R4-ospf-1]area 0
[R4-ospf-1-area-0.0.0.0]network  4.4.4.4 0.0.0.0
[R4-ospf-1-area-0.0.0.0]network 34.1.1.0 0.0.0.255
[R4-ospf-1-area-0.0.0.0]network 24.1.1.0 0.0.0.255
[R4]bgp 200
[R4-bgp-default]router-id 4.4.4.4
[R4-bgp-default]peer 3.3.3.3 as-number 200
[R4-bgp-default]peer 2.2.2.2 as-number 200
[R4-bgp-default]peer 2.2.2.2 con l0
[R4-bgp-default]add ipv4
[R4-bgp-default-ipv4]peer 2.2.2.2 enable 
[R4-bgp-default-ipv4]peer 3.3.3.3 enable 
[R4-bgp-default-ipv4]network  4.4.4.4 32
[R4-bgp-default-ipv4]network  24.1.1.0 24
[R4-bgp-default-ipv4]network  34.1.1.0 24
```
</br></br></br>

#### 配置完基本配置后，在R1上查询bgp路由表，并ping 测试4.4.4.4且加上-r参数看数据的路径
![](/images/router_1_02.jpg)
![](/images/router_1_03.jpg)

#### 由此可见R1去往4.4.4.4网络默认的路径是从R2走的；则现在需要在R1上修改preferred-value属性(<font color="green">该属性值默认为0，越大越优先，该实验中被修改成了2</font>)，使其R1去往4.4.4.4网络的路径从下面经过R3走。

 - **则在R1上配置**

```
[R1]ip prefix-list qzk permit 4.4.4.4 32
[R1]route-policy qzk permit  node 10
[R1-route-policy-qzk-10]if-match  ip address prefix-list qzk
[R1-route-policy-qzk-10]apply  preferred-value 2
[R1-route-policy-qzk-10]qu
[R1]route-policy qzk permit  node 20
[R1-route-policy-qzk-20]qu
[R1]bgp 100
[R1-bgp-default]add ipv4
[R1-bgp-default-ipv4]peer 13.1.1.2 route-policy qzk import
```

#### 修改完preferred-value属性后再次查询R1的bgp路由表，并用R1 ping 4.4.4.4 加上-r参数。

![](/images/router_1_04.jpg)
![](/images/router_1_05.jpg)

#### 此时preferred-value属性修改成功，R1去往4.4.4.4网络的路径也被修改成功。

</br>

### BGP路由优先

![](/images/router_1_06.jpg)