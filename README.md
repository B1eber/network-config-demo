# network-config-demo
交换机的一些配置（方便重复记忆）
```bash
CS(config)#router ospf 1
ospf router-id 10.5.0.254
 network 10.5.0.252 0.0.0.3 area 0
 network 10.6.0.252 0.0.0.3 area 0
 no shutdown
```
