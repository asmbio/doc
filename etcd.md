#### 启动脚本
注意修改ip 地址和端口

```
etcd -name e0 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:2380 \
          -listen-peer-urls http://0.0.0.0:2380 \
            -listen-client-urls http://0.0.0.0:2379 \
              -advertise-client-urls http://0.0.0.0:2379 \
                -initial-cluster e0=http://0.0.0.0:2380,e1=http://0.0.0.0:23801,e2=http://0.0.0.0:23802 \
                -initial-cluster-state new
```

```
etcd -name e1 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:23801 \
          -listen-peer-urls http://0.0.0.0:23801 \
            -listen-client-urls http://0.0.0.0:23791 \
              -advertise-client-urls http://0.0.0.0:23791 \
                -initial-cluster e0=http://0.0.0.0:2380,e1=http://0.0.0.0:23801,e2=http://0.0.0.0:23802 \
                -initial-cluster-state new
```

```
etcd -name e2 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:23802 \
          -listen-peer-urls http://0.0.0.0:23802 \
        -listen-client-urls http://0.0.0.0:23792 \
              -advertise-client-urls http://0.0.0.0:23792 \
                -initial-cluster e0=http://0.0.0.0:2380,e1=http://0.0.0.0:23801,e2=http://0.0.0.0:23802 \
                -initial-cluster-state new                                          
```

```
etcd -name e3 -initial-advertise-peer-urls http://0.0.0.0:23803 \
          -listen-peer-urls http://0.0.0.0:23803 \
            -listen-client-urls http://0.0.0.0:23793 \
              -advertise-client-urls http://0.0.0.0:23793 \
               -initial-cluster e0=http://0.0.0.0:2380,e2=http://0.0.0.0:23802,e1=http://0.0.0.0:23801,,e3=http://0.0.0.0:23803 \
                -initial-cluster-state existing
```
单节点 启动脚本

```
etcd -name e0 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:2380 \
          -listen-peer-urls http://0.0.0.0:2380 \
            -listen-client-urls http://0.0.0.0:2379 \
              -advertise-client-urls http://0.0.0.0:2379 \
                -initial-cluster e0=http://0.0.0.0:2380 \
                -initial-cluster-state new
```

#### 设置访问权限

```

etcdctl user add root
etcdctl user add reader
etcdctl role add reader
etcdctl role grant-permission reader read asmb/ asmb~
etcdctl user grant reader reader
etcdctl auth enable

```

取消访问权限

```
etcdctl --user root:123456 auth disable
```

参考:https://blog.csdn.net/u013761036/article/details/103900225

