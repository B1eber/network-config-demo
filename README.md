# network-config-demo
交换机的一些配置（方便重复记忆）

```shell
ospf
CS(config)#router ospf 1
ospf router-id 10.5.0.254
 network 10.5.0.252 0.0.0.3 area 0
 network 10.6.0.252 0.0.0.3 area 0
```
```shell
查看路由表
WS#show ip route database
```
```shell
查看邻居
WS#show ip ospf neighbor 

OSPF process 1:
Neighbor ID     Pri   State           Dead Time   Address         Interface
10.5.0.254        1   Full/DR         00:00:36    10.5.0.254      Vlan115
10.5.0.254        1   Full/DR         00:00:36    10.6.0.254      Vlan116
```
