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
