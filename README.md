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


