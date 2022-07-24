# 启动须知

> 创建一个3个主节点3个副本的redis集群

1. redis cluster 至少需要3个实例才能使用
2. 要把 conf 下的配置文件中`cluster-announce-ip` 更换为宿主机的ip
3. 启动之后， 在启动之后，需要进入其中一个实例，并执行创建cluster的命令
```shell
# 在所有redis 启动之后 创建cluster 副本为1
redis-cli --cluster create 192.168.1.5:6371 192.168.1.5:6372 192.168.1.5:6373 192.168.1.5:6374 192.168.1.5:6375 192.168.1.5:6376 --cluster-replicas 1
```
控制台输出
```plain
# redis-cli --cluster create 192.168.1.5:6371 192.168.1.5:6372 192.168.1.5:6373 192.168.1.5:6374 192.168.1.5:6375 192.168.1.5:6376 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 192.168.1.5:6375 to 192.168.1.5:6371
Adding replica 192.168.1.5:6376 to 192.168.1.5:6372
Adding replica 192.168.1.5:6374 to 192.168.1.5:6373
>>> Trying to optimize slaves allocation for anti-affinity
[WARNING] Some slaves are in the same host as their master
M: c33f04b1b44569d7a95c7ad6282e09ec601cc2d1 192.168.1.5:6371
   slots:[0-5460] (5461 slots) master
M: f91fe77645ed36988e887e91d324273b076793e7 192.168.1.5:6372
   slots:[5461-10922] (5462 slots) master
M: b6e61f9f3d2a6a86742b8362f34ae3c091b4b5f6 192.168.1.5:6373
   slots:[10923-16383] (5461 slots) master
S: 85b78039166d4c72a68b4834432a71918025e2f6 192.168.1.5:6374
   replicates b6e61f9f3d2a6a86742b8362f34ae3c091b4b5f6
S: 7fadf372088a387434920a7a0b7f8efb8add01b8 192.168.1.5:6375
   replicates c33f04b1b44569d7a95c7ad6282e09ec601cc2d1
S: df3042d2ae6282c625c949c1c8ec97c1adae8528 192.168.1.5:6376
   replicates f91fe77645ed36988e887e91d324273b076793e7
Can I set the above configuration? (type 'yes' to accept):
# 如果同意上面的配置， 可以输入yes
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 192.168.1.5:6371)
M: c33f04b1b44569d7a95c7ad6282e09ec601cc2d1 192.168.1.5:6371
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 85b78039166d4c72a68b4834432a71918025e2f6 192.168.1.5:6374
   slots: (0 slots) slave
   replicates b6e61f9f3d2a6a86742b8362f34ae3c091b4b5f6
S: 7fadf372088a387434920a7a0b7f8efb8add01b8 192.168.1.5:6375
   slots: (0 slots) slave
   replicates c33f04b1b44569d7a95c7ad6282e09ec601cc2d1
S: df3042d2ae6282c625c949c1c8ec97c1adae8528 192.168.1.5:6376
   slots: (0 slots) slave
   replicates f91fe77645ed36988e887e91d324273b076793e7
M: b6e61f9f3d2a6a86742b8362f34ae3c091b4b5f6 192.168.1.5:6373
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
M: f91fe77645ed36988e887e91d324273b076793e7 192.168.1.5:6372
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

# 到这里就是创建主节点为3个副本为3个的redis 集群
```

```shell
# 检查cluster状态
redis-cli  --cluster check 192.168.1.5:6371

# redis-cli  --cluster check 192.168.1.5:6371
192.168.1.5:6371 (c33f04b1...) -> 0 keys | 5461 slots | 1 slaves.
192.168.1.5:6373 (b6e61f9f...) -> 0 keys | 5461 slots | 1 slaves.
192.168.1.5:6372 (f91fe776...) -> 0 keys | 5462 slots | 1 slaves.
[OK] 0 keys in 3 masters.
0.00 keys per slot on average.
>>> Performing Cluster Check (using node 192.168.1.5:6371)
M: c33f04b1b44569d7a95c7ad6282e09ec601cc2d1 192.168.1.5:6371
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 85b78039166d4c72a68b4834432a71918025e2f6 192.168.1.5:6374
   slots: (0 slots) slave
   replicates b6e61f9f3d2a6a86742b8362f34ae3c091b4b5f6
S: 7fadf372088a387434920a7a0b7f8efb8add01b8 192.168.1.5:6375
   slots: (0 slots) slave
   replicates c33f04b1b44569d7a95c7ad6282e09ec601cc2d1
S: df3042d2ae6282c625c949c1c8ec97c1adae8528 192.168.1.5:6376
   slots: (0 slots) slave
   replicates f91fe77645ed36988e887e91d324273b076793e7
M: b6e61f9f3d2a6a86742b8362f34ae3c091b4b5f6 192.168.1.5:6373
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
M: f91fe77645ed36988e887e91d324273b076793e7 192.168.1.5:6372
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```


# 常见问题

## redis集群Waiting for the cluster to join一直在等待
原因是：
redis集群总线不通， 总线的端口是客户端连接的端口+10000

例如：端口6379 那么集群总线端口是16379

在docker环境中，要保证每个容器之间要能通过总线互相访问

## Redis集群Hash槽分配异常 CLUSTERDOWN Hash slot not served

这个的原因是，如果开启了redis的集群模式，又没有加入集群那么就会产生这种现象
```shell
# redis-cli -p 6371
127.0.0.1:6371> set name 1212
(error) CLUSTERDOWN Hash slot not served
```


