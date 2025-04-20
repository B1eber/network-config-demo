# network-config-demo
交换机的一些配置（方便重复记忆）
<details>
<summary>ospf</summary>

```shell
CS(config)#router ospf 1
 ospf router-id 10.5.0.254
 network 10.5.0.252 0.0.0.3 area 0
 network 10.6.0.252 0.0.0.3 area 0
```
</details>

<details>
<summary>查看路由表</summary>

```shell
WS#show ip route database
```
</details>
<details>
<summary>查看邻居</summary>

```shell
WS#show ip ospf neighbor 

OSPF process 1:
Neighbor ID     Pri   State           Dead Time   Address         Interface
10.5.0.254        1   Full/DR         00:00:36    10.5.0.254      Vlan115
10.5.0.254        1   Full/DR         00:00:36    10.6.0.254      Vlan116
```
</details>

<details>
<summary>链路聚合trunk</summary>

```shell
CS(config)#port-group 1
#
CS(config-if-port-range)#port-group 1 mode active
CS(config)#int port-channel 1
CS(config-if-port-channel1)#switchport trunk allowed vlan 113,114

```
</details>
<details>
<summary>查看端口状态</summary>

```shell
CS#show ip interface brief 
```
</details>
<details>
<summary>FW配置ospf</summary>

```shell
FW(config)# ip vrouter trust-vr 
FW(config-vrouter)# router ospf 1
FW(config-router)# router-id 10.1.0.254
FW(config-router)# network 10.1.0.254/30 area 0
FW(config-router)# network 10.2.0.254/30 area 0
```
</details>
<details>
<summary>开启路由转发</summary>

```shell
Switch(config)#ip routing  (开启路由转发，只有思科和锐捷有，神州数码、华为、华三没有)
```
</details>
<details>
<summary>ap上线</summary>

```shell
AP基本配置命令

AP登录用户名和密码均为：admin

AP默认IP地址为：192.168.1.10

AP默认情况下DHCP开启

AP静态地址配置：

set management static-ip 192.168.10.1

AP 开启/关闭DHCP功能：

set management dhcp-status up/down

AP设置默认网关：

set static-ip-route geteway 192.168.10.254

查看AP基本信息：

get system

get management

get managed-ap

get route

AP配置管理

AP注册管理

无线控制器开启无线功能

二层模式

三层模式

AC常用功能配置方法

配置架构

SSID设置

用户vlan设置

无线加密设置

转发模式设置

射频管理

AC上开启无线功能（必选）

AC上无线功能默认是关闭的，AC能够管理AP的前提是开启AC

的无线功能

开启无线功能的条件：AC上有合法的无线IP地址

无线ip地址的来源：AC上面loopback接口或者UP的三层接口的IP地址

无线IP地址的配置方式：动态选取和静态指定

动态选取的优先级高于静态指定的ip优先级（实际项目中建议使用静态指定）

动态选取无线IP地址（可选）

动态选取无线ip地址的原则：

优先选择接口ID小的Loopback接口的地址

未配置Loopback接口时，优先选择接口ID小的三层接口IP地址（注意不是IP地址小的接口）

静态指定无线IP地址（必选）

设置静态的无线IP地址：

6028(config-wireless)#static-ip 192.168.1.254

关闭无线ip地址的自动选取功能：

6028(config-wireless)#no auto-ip-assign

查看AC选取的无线ip地址：

6028#show wireless

WS IP Address.................................. 192.168.1.254

WS Auto IP Assign Mode ........................ Disable

WS Switch Static IP ........................... 192.168.1.254

建议项目实施时采用静态指定无线IP地址的方式，防止动态选取时IP地址变化导致无线网络中断

AP注册方式

AP工作在瘦模式时需要注册到AC上，成功注册后才能接受AC的统一管理。

有两种注册方式：AC发现AP、AP发现AC

AC发现AP有两种模式：二层发现模式、三层发现模式

AP发现AC有两种方式：AP上静态指定AC列表、AP通过DHCP方式获取AC列表（利用option 43选项）

#项目实施时建议采用AC二层发现AP方式（AC、AP二层连通）或者利用DHCP Option 43方式让AP发现AC（AC、AP三层连通）。

注意：

不论使用何种注册方式，AP要成功注册到AC上的前提是：

AC的无线IP地址和AP的IP地址三层可达

即：AP的IP地址和AC的无线地址能够ping通。

AC二层发现AP

概念：AC通过二层广播发现报文方式来发现vlan内的AP。

要求：AC和AP在同一个二层网络中。

基本原理：AC上面可以指定二层自动发现的vlan列表，向列表中的各个vlan发送自动发现报文。收到广播发现报文的AP会向AC做出响应。

注意：只有AC发出的Discovery报文是广播的，后续AC-AP间交互的报文均是单播（udp）。

该方式适用于二层部署。

配置方法：

AC上配置：

开启AC无线功能

6028(config)#wireless

6028(config-wireless)#enable

指定vlan发现列表

6028(config-wireless)# discovery vlan-list 10

Vlan1是默认加入vlan-list的，可以删除。

6028(config-wireless)# no discovery vlan-list 1

AC三层发现AP

AC根据AP的IP地址单播发现AP的方式

AC上面配置三层发现IP地址列表，AC根据列表中的地址逐个发现AP。

配置方法：

将AP的IP地址加入到三层发现IP列表

6028(config-wireless)#discovery ip-list 192.168.2.10

查看已配置的IP发现列表

6028#show wireless discovery ip-list

IP Address Status

--------------- ------------------

192.168.2.10 Discovered

AP注册认证方式

AP注册到AC时，AC需要对AP进行认证

主要认证方式：

MAC认证：通过检查AP的mac地址来决定AP是否能够注册到AC上。默认的认证方式。AC上面通过ap database来添加AP的mac地址。大规模部署时比较麻烦。

None：免认证，即AP自动注册，便于部署。推荐使用这种方式。

Pass-Phase认证：密码认证，AC和AP比对密码，密码一致时AP可以注册到AC上。AP需要做配置，使用不便，用的较少。

AP认证方式配置方法

MAC认证：

6028(config-wireless)#ap database xx-xx-xx-xx-xx-xx

AP自动注册：

6028(config-wireless)#ap authentication mode none

配置了AP自动注册后，AC上面会自动添加AP的mac地址（对AP个体的配置均要在database模式下操作并重启AP，后面介绍）

AP主动发现AC

AP主动发现AC只能通过AC的无线IP地址进行单播发现

按照AP上面获取AC地址方式的不同，可以分为一下几种发现方式：

AP通过静态AC地址主动发现AC（必须掌握）

AP通过DHCP Option43获取AC地址并发现（必须掌握）

AP通过AC下发的自动部署地址发现AC（暂不讲解）

AP上面配置静态AC地址（最多配置4个）

AP通过DHCP Option43获取AC地址

DHCP Option43和Option60配合使用。Option60为厂商标识字段，如“udhcp 1.18.2”。如果不确定，可以通过抓取AP发出的DHCP Discovery报文来查看。Option43为自定义字段，这里将AC的无线地址作为Option43的内容下发给AP。DHCP服务器会同时配置这两个选项，只有AP发出的Option60与服务器一致，服务器才会回应Option43。

注意：

1、AP必须使用动态获取地址的方式。

2、Option43并不影响AP自身正常获取地址。如果Option60与服务器匹配失败，AP本身是可以获取IP地址的，只是服务器回应的报文不包含Option43。

3、DCN的交换机支持下发Option43，客户自己的服务器需要确认是否支持该功能。

几种方式使用建议：

AP部署数量少的情况下，两种方式均可以考虑；

AP部署数量多的情况下，建议使用DHCP Option43的方式，可以做到AP的“零配置上线”。

AP注册状态查看

managed： AP处于管理状态

failed： AP未处于管理状态

AP配置下发

瘦AP的配置由AC进行下发，所有功能在AC上面配置。

熟练掌握：

AC对AP整体配置架构

主要功能的配置方法

配置架构图：

配置结构说明：

每个AP关联一个profile，默认关联到profile 1上。

network 1-1024为全局公共配置。对于AP而言，每个VAP都唯一对应一个network，AC上面默认有16个network（1-16），与vap的0-15对应。

radio 1对应AP上2.4Ghz工作频段，radio 2对应AP上5Ghz工作频段。

更改全局或profile的配置，都要将profile下发一次，下发命令是：wireless ap profile apply X

X表示profile序号，所有应用这个profile的AP都会更新配置。

每次AP注册到AC上时，AC会自动下发profile配置

AP与profile对应关系设置

把AP与某个profile绑定（需要重启AP）

DCWS-6028#

DCWS-6028#config

DCWS-6028(config)#wireless

DCWS-6028(config-wireless)#ap database 00-03-0f-58-80-00

DCWS-6028(config-ap)#profile 1

DCWS-6028(config-ap)#

DCWS-6028#

设置profile对应的硬件类型

DCWS-6028(config)#wireless

DCWS-6028(config-wireless)#ap profile 1

DCWS-6028(config-ap-profile)#hwtype 7

DCWS-6028(config-ap-profile)#

SSID设置

设置SSID步骤为：

6028(config-wireless)#network 1

6028(config-network)#ssid dcn_wlan

下发profile配置：

6028#wireless ap profile apply 1

每个network下可设置一个SSID，多SSID情况下需要使用多个network

6028(config-wireless)#network 2

6028(config-network)#ssid guest_wlan

radio下需要启用对应的vap：

6028(config-ap-profile)#radio 1

6028(config-ap-profile-radio)#vap 1

6028(config-ap-profile-vap)#enable

注意：

注意：

AP上面VAP0是始终开启的，且不能关闭。所以在不同AP广播不同SSID的应用场景下，不要使用Network1配置SSID，并将Network1的默认SSID隐藏！

DCWS-6028(config-wireless)#network 1

DCWS-6028(config-network)#hide-ssid

DCWS-6028(config-network)#

无线加密设置

open方式（默认）

6028(config-wireless)#network 1

6028(config-network)#security mode none

设置加密方式为WPA个人版， WPA version可以设置为wpa、wpa2以及wpa/wpa2混合模式，默认为wpa/wpa2混合模式，此处加密密钥为12345678

6028(config-wireless)#network 1

6028(config-network)#security mode wpa-personal

6028(config-network)#wpa key 12345678

设置加密方式为WPA企业版，此方式需要使用radius认证

6028(config-wireless)#network 1

6028(config-network)#security mode wpa-enterprise

认证计费设置

6028(config)#radius-server authentication host 192.168.1.250

6028(config)#radius-server accounting host 192.168.1.250

6028(config)#radius-server key dcn

6028(config)#radius nas-ipv4 192.168.1.254 //与无线IP相同

6028(config)#radius source-ipv4 192.168.1.254 //与无线IP相同

6028(config)#aaa enable

6028(config)#aaa-accounting enable

6028(config)#aaa group server radius wlan

6028(config-sg-radius)#server 192.168.1.250

network下关联aaa group：

6028(config-wireless)#network 1

6028(config-network)#radius server-name auth wlan

6028(config-network)#radius server-name acct wlan

6028(config-network)#radius accounting

WLAN设置

在network下指定vlan信息，当用户接入network时，就属于该network所在的vlan，数据将在相应vlan内转发。

6028(config-wireless)#network 1

6028(config-network)#vlan 10

6028(config-network)#exit

6028(config-wireless)#network 2

6028(config-network)#vlan 20

本地转发

本地转发是默认的转发方式，AC上不需配置，遵守二/三层转发的一般规则。其转发的主体是AP，AC只是根据需要做一般的路由交换。对于常见的AC旁挂方式，AC几乎不参与用户数据的转发。

集中式转发

AC和AP之间建立集中式隧道，所有数据均通过隧道传送至AC进行后续处理和转发。要求AC及AC对端互联的交换机上面必须配置有各个用户的数据vlan，且两者互联端口为trunk，否则隧道报文在解封装后无法转发！

AP注册成功后会自动建立一条隧道，如下提示：

Interface capwaptnl1, changed state to administratively UP

注意：隧道是自动建立，但此时的隧道并不具备转发数据的功能。

集中式转发是依据vlan进行的。

向集中式隧道的vlan-list中添加vlan，相应vlan的数据就会在集中式隧道内转发。

集中式转发配置

把数据vlan加入vlan-list里

DCWS-6028(config-wireless)#l2tunnel vlan-list 10

DCWS-6028(config-wireless)#l2tunnel vlan-list 20

查看当前vlan-list包含的vlan列表：

DCWS-6028(config)#show wireless l2tunnel vlan-list

射频管理

调整射频工作模式：

6028(config-wireless)#ap profile 1

6028(config-ap-profile)#radio 1

6028(config-ap-profile-radio)#mode bg-n

设置AP的静态功率和信道，调整完需重启AP生效

6028(config-wireless)#ap database 00-03-0f-19-71-e0

6028(config-ap)#radio 1 channel 11

The valid AP entry is updated. This AP is already managed, to update the managed AP configuration with the new value(s) you need to reset the AP.

6028(config-ap)#radio 1 power 50


```
</details>
<details>
<summary>dhcpsnoopingbinding</summary>

```shell
//config模式下
ip dhcp snooping enable
ip dhcp snooping binding enable
//端口下
Interface Ethernet1/0/11
 switchport access vlan 10
 ip dhcp snooping binding user-control
!
Interface Ethernet1/0/12
 switchport access vlan 20
 ip dhcp snooping binding user-control
```
</details>
<details>
<summary>DHCP中继</summary>

```shell
//config模式下
ip forward-protocol udp bootps //允许udp数据通过
//端口下
ip helper-address dhcp  //服务地址也可以是对端端口地址
如果要有中继情况下在加上DHCPsnoopingbinding则需要在链接端dhcp的端口上加上：
ip dhcp snooping trust //信任



