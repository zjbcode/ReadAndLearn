
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->
<!-- code_chunk_output -->

* [2 基础网络概念](#2-基础网络概念)
	* [2.1 网络是个什么玩意儿](#21-网络是个什么玩意儿)
		* [2.1.4 计算机网络协议： OSI 七层协定](#214-计算机网络协议-osi-七层协定)
			* [七层结构[1]](#七层结构1)
			* [五种中继系统[2]](#五种中继系统2)
			* [协议数据单元PDU[1]](#协议数据单元pdu1)
	* [2.2 TCP/IP的链结层相关协议](#22-tcpip的链结层相关协议)
		* [2.2.1 广域网使用的设备](#221-广域网使用的设备)
		* [2.2.3 以太网络的传输协议： CSMA/CD](#223-以太网络的传输协议-csmacd)
			* [集线器](#集线器)
		* [2.2.4 MAC 的封装格式](#224-mac-的封装格式)
		* [2.2.6 集线器、交换器与相关机制](#226-集线器-交换器与相关机制)
	* [2.3 TCP/IP的网络层相关封包与数据](#23-tcpip的网络层相关封包与数据)
		* [2.3.1 IP封包的封装](#231-ip封包的封装)
		* [2.3.2 IP 地址的组成与分级](#232-ip-地址的组成与分级)
		* [2.3.4 Netmask, 子网与 CIDR (Classless Interdomain Routing)](#234-netmask-子网与-cidr-classless-interdomain-routing)
		* [2.3.5 路由概念](#235-路由概念)
		* [2.3.6 观察主机路由： route](#236-观察主机路由-route)
		* [2.3.7 IP 与 MAC：链结层的 ARP 与 RARP 协定](#237-ip-与-mac链结层的-arp-与-rarp-协定)
		* [2.3.8 ICMP 协定](#238-icmp-协定)
	* [2.4 TCP/IP 的传输层相关封包与数据](#24-tcpip-的传输层相关封包与数据)
		* [2.4.1 可靠联机的 TCP 协议](#241-可靠联机的-tcp-协议)
		* [2.4.2 TCP 的三向交握](#242-tcp-的三向交握)
		* [2.4.3 非连接导向的 UDP 协议](#243-非连接导向的-udp-协议)
		* [2.4.4 网络防火墙与 OSI 七层协议](#244-网络防火墙与-osi-七层协议)
	* [2.5 连上 Internet 前的准备事项](#25-连上-internet-前的准备事项)
		* [2.5.1 用 IP 上网？主机名上网？ DNS 系统？](#251-用-ip-上网主机名上网-dns-系统)
		* [2.5.2 一组可以连上 Internet 的必要网络参数](#252-一组可以连上-internet-的必要网络参数)

<!-- /code_chunk_output -->
---

# 2 基础网络概念


## 2.1 网络是个什么玩意儿

### 2.1.4 计算机网络协议： OSI 七层协定

#### OSI七层协议（Open System Interconnection）

|具体7层|数据格式|功能与连接方式|典型设备|
|-|-|-|-|
|应用层 Application|数据Data|网络服务与使用者应用程序间的一个接口|终端设备（PC、手机、平板等）|
|表示层 Presentation|数据Data|数据表示、数据安全、数据压缩|终端设备（PC、手机、平板等）|
|会话层 Session|数据Data|会话层连接到传输层的映射；会话连接的流量控制；数据传输；会话连接恢复与释放；会话连接管理、差错控制|终端设备（PC、手机、平板等）|
|**传输层** Transport|数据组织成数据段Segment|用一个寻址机制来标识一个特定的应用程序（端口号）|终端设备（PC、手机、平板等）|
|**网络层** Network|分割和重新组合数据包Packet|基于网络层地址（IP地址）进行不同网络系统间的路径选择|**网关、路由器**|
|**数据链路层** Data Link|将**比特**信息封装成**数据帧Frame**|在物理层上建立、撤销、标识逻辑链接和链路复用 以及差错校验等功能。通过使用接收系统的硬件地址或物理地址来寻址|**网桥、交换机**|
|**物理层**Physical|**传输比特（bit）流**|建立、维护和取消物理连接|光纤、同轴电缆、|**双绞线、网卡、中继器、集线器**|


#### 五种中继系统[2]

* **物理层**
  * （即常说的第一层、层Ｌ1）中继系统，即转发器（repeater）。
  * 网卡，网线，集线器，中继器，调制解调器。
  * **比特**
* **数据链路层**
  * （即第二层，层Ｌ2），即**网桥**或桥接器（bridge）。
  * 网桥，交换机。
  * **MAC**(Media Access Control)，**数据帧**。
  * 该层通常又被分为介质访问控制（MAC）和逻辑链路控制（LLC）两个子层
    * MAC子层的主要任务是解决共享型网络中多用户对信道竞争的问题，完成网络介质的访问控制
    * LLC子层的主要任务是建立和维护网络连接，执行差错校验、流量控制和链路控制。
* **网络层**
  * （第三层，层Ｌ3）中继系统，即**路由器**（router）。
  * 网桥和路由器的混合物桥路器（brouter）兼有网桥和路由器的功能。
  * 路由器。**IP**(Internet Protocol)，**数据包**。
* **传输层**。
  * 在网络层以上的中继系统，即**网关（gateway）**。
  * 网关工作在第四层传输层及其以上。
  * TCP，UDP，**数据段**。

#### 协议数据单元PDU[1]
OSI参考模型中，对等层协议之间交换的信息单元统称为协议数据单元(PDU，Protocol Data Unit)。而传输层及以下各层的PDU另外还有各自特定的名称：

* 传输层——数据段（Segment）
* 网络层——分组（数据包）（Packet）
* 数据链路层——数据帧（Frame）
* 物理层——比特（Bit）

## 2.2 TCP/IP的链结层相关协议


### 2.2.1 广域网使用的设备

以太网络线有八蕊且两两成对，但实际使用的只有1,2,3,6蕊

### 2.2.3 以太网络的传输协议： CSMA/CD

IEEE 802.3标准CSMA/CD（Carrier Sense Multiple Access with Collision Detection）

#### 集线器

* 集线器
  * 集线器是一种网络共享媒体；
  * 网络共享媒体在单一时间点内，仅能被一部主机所使用；
  * 不管哪一部主机发送讯框，集线器会复制一份该数据给所有计算机；

### 2.2.4 MAC 的封装格式


### 2.2.6 集线器、交换器与相关机制

* 全双工/半双工
  * 八蕊的网络仅有两对被使用，一对传送，一对接收
  * 网络环境达到全双工：PC端支持全栓双工，同时switch也支持全双工
  * 共享媒体的Hub不支持全双工

## 2.3 TCP/IP的网络层相关封包与数据

### 2.3.1 IP封包的封装

IPv4(Internet Protocol version 4，因特网协定第四版)
IP封包可达 **65535** bytes

* 表头
  * Version（版本）
  * IHL（Internet Header Length，IP表头的长度）：IP封包的表头长度
  * Type of Service（服务类型）**PPPDTRUU**
    * PPP：IP封包的优先度
    * D：0一般延迟，1低延迟
    * T：0一般传输量，1高传输量
    * R：0一般可靠度，1高可靠度
    * UU：保留尚未被使用
  * Total Length（总长度）：IP封包的总容量，包括表头和内容。最大可达 **65535** bytes
  * Identification（辨别码）
    * **IP封包必须要放在MAC封包中**，
    * 如果IP太大，先要将IP重组成较小的数据然后再放到MAC当中
    * 通过Identification告知接收端，这些数据来自同一个IP封包
  * Flags（特殊旗标），内容为0DM
    * D：0可分段，1不可分段
    * M：0此IP最后分段，1非最后分段
  * Fragment Offset（分段偏移）
    * 当前IP分段在原始IP封包中所占的位置
    * 将所有的IP分段组合成原本的IP封包
    * 通过Total Length, Identification, Flags以及Fragment Offset将IP分段在接收端组合起来
  * Time To Live（TTL，存活时间）
    * IP封包的存活时间，范围为0~255
    * IP封包通过一个路由器，TTL减一
    * 当TTL为0，直接丢弃封包
  * Protocol Number（协定代码）：TCP、UDP、ICMP等
  * Header Checksum（表头检查码）
  * Source Address（来源IP地址）
  * Destination Address（目标IP地址）
  * Options（其他参数）
  * Padding（补齐项目） 
  
### 2.3.2 IP 地址的组成与分级

* IP：
  * IP(Internet Protocol)是一种网络封包；
  * 封包的表头最重要的就是32位的来源与目的地址；
  * 32 bits的IP分成四小段，每段含有8个bits；
  * 主要分为Net_ID(网域号码)与Host_ID(主机号码)两部分；
* 网域：  
  * **网域**的定义：在同一个物理网段内，主机的IP具有相同的Net_ID，并且具有独特的Host_ID；
  * 同网段内，Net_ID不变，Host_ID不重复；
  * Host_ID不可同时为0也不可同时为1；
  * Host_ID全为0表示整个网段的地址Network IP；
  * Host_ID全为1表示广播的地址Broadcast IP；
  * 同网段主机可通过CSMA/CD广播通信，也可通过MAC直接通信；


### 2.3.4 Netmask, 子网与 CIDR (Classless Interdomain Routing)


### 2.3.5 路由概念

* 数据广播
  * 在同一个区网中，可以通过IP广播的方式来达到资料传递的目的
  * 非区网内的数据需要通过路由器
* 路由：
  * Gateway/Router：网关/路由器的功能就是在负责不同网域之间的封包传递（IP Forwarding）.
  * 封包进过路由器后，将有路由器中的**路由表**来决定发送目的地。
* 路由过程
  * 查询IP封包的目标IP地址
  * 查询是否位于本机所在的网域
  * 不在同一网域，查询路由表是否有相符的路由设定；如果没有，则将IP封包送到预设路由器
  * 路由器收到封包后，依据上述流程，分析自己的路由信息，继续传输到正确的主机上

### 2.3.6 观察主机路由： route

### 2.3.7 IP 与 MAC：链结层的 ARP 与 RARP 协定

* ARP & RARP
  * ARP（Address Resolution Protocol，网络地址解析协议）
  * RARP（Revers ARP，反向网络地址解析协议）
* ARP
  * 主机对整个区网发送出ARP封包
  * 对方收到ARP封包后，回传MAC地址
  * 取得目标IP与MAC地址后，写入到主机ARP table中记录20分钟

### 2.3.8 ICMP 协定

* ICMP（Internet Control Message Protocol，因特网信息控制协议）
  * 错误侦测与回报的机制
  * ICMP透过IP封包进行数据传送
  * IP封包有传输能力
  * ping与traceroute透过ICMP封包来确认与回报网络主机的状态

## 2.4 TCP/IP 的传输层相关封包与数据

### 2.4.1 可靠联机的 TCP 协议

* IP
  * 网络层的IP封包**只负责将数据送到正确的目标主机**
  * 封包会不会被接收不是IP的任务
* TCP表头
  * Source Port & Destination Port来源端口 & 目标端口
  * Sequence Number封包序号
    * TCP数据太大时，将TCP数据分段
    * Sequence Number记录封包的序号
    * 接收端按照封包序号将TCP数据组合起来
  * Acknowledge Number回应序号：服务端收到传递的封包，发送给客户端的确认码
  * Data Offset资料补偿：封包区段的起始位置，确认整个TCP封包的大小
  * Code（Control Flag，控制标志码）：共有6个bits，代表6个句柄，1为启动
    * URG(Urgent)紧急封包
    * ACK(Acknowledge)响应封包
    * PSH(Push function)立即传送缓存区内封包
    * RST(Reset)立即终止
    * SYN(Synchronous)同步请求
    * FIN(Finish)通知传送完毕
  * Window滑动窗口：目前本身有的缓冲器容量(Receive Buffer)还可以接收封包
  * Checksum(确认检查码)：发送时进行检验生成检验值，接收者收到封包再次验证，并对比Checksum值
  * Urgent Pointer(紧急资料)：Code中URG=1时生效，告知紧急数据的位置
  * Options(任意资料)：接收端可以接收的最大数据区容量
  * Padding(补足字段)：补齐Options字段
* 端口
  * 网络是双向的

|端口| 服务名称与内容                                  |
|----|-----------------------------------------------|
|20  |FTP-data，文件传输协议所使用的主动数据传输端口口    |
|21  |FTP，文件传输协议的命令通道                       |
|22  |SSH，较为安全的远程联机服务器                      |
|23  |Telnet，早期的远程联机服务器软件                   |
|25  |SMTP，简单邮件传递协议，用在作为 mail server 的埠口  |
|53  |DNS，用在作为名称解析的领域名服务器                 |
|80  |WWW，这个重要吧！就是全球信息网服务器                |
|110 |POP3，邮件收信协议，办公室用的收信软件都是透过他      |
|443 |https，有安全加密机制的 WWW 服务器                  |

### 2.4.2 TCP 的三向交握

* 三次握手(Three-way handshake)
  * 封包发送：客户端使用一个大于1024端口，TCP表头添加SYN=1，记下联机序号Sequence (seq=10001)
  * 封包接收与封包确认
    * 服务端收到封包，制作同时带有SYN=1, ACK=1的封包
    * ACK给客户端确认信息，Sequence号码多一位(ack = 10001 + 1 = 10002)
    * SYN服务器端确认客户端能接收封包，Sequence (seq=20001)
  * 回送确认封包
    * ACK再次发送确认封包(ack = 20001 + 1 = 20002)
  * 服务端收到ACK=1且ack=20002后，开始发送数据

### 2.4.3 非连接导向的 UDP 协议

* UDP(User Datagram Protocol，用户数据流协议)
  * 接收端在接收封包后，不会回复响应封包(ACK)给发送端

### 2.4.4 网络防火墙与 OSI 七层协议

* 防火墙
  * 第二层，来源和目标的MAC
  * 第三层，来源和目标的IP，ICMP类别(type)
  * 第四层，TCP/UDP端口，TCP状态(code)

## 2.5 连上 Internet 前的准备事项

### 2.5.1 用 IP 上网？主机名上网？ DNS 系统？

* DNS
  * Domain Name System(DNS)主机名(Hostname)对应IP系统
  * DNS服务器配置：/etc/resolv.conf

### 2.5.2 一组可以连上 Internet 的必要网络参数

Network与Broadcast可以经由IP/Network计算而得
设置IP, Netmask, Default Gateway, DNS四个参数

* IP：由192.168.1.1 ~ 192.168.1.254
* Netmask：255.255.255.0
* Network：192.168.1.0
* Broadcast：192.168.1.255
* Gateway
* DNS
