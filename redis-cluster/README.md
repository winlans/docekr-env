# 启动须知

> 创建一个3个主节点的redis集群
> 
1. redis cluster 至少需要3个实例才能使用
2. 要把 conf 下的配置文件中`cluster-announce-ip` 更换为宿主机的ip
3. 启动之后， 在启动之后，需要进入其中一个实例，并执行创建cluster的命令
```shell
# 在所有redis 启动之后 创建cluster
redis-cli --cluster create 192.168.1.5:6371 192.168.1.5:6372 192.168.1.5:6373
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


