#### 下载安装

https://github.com/etcd-io/etcd/releases/

解压到 c:/asmb/bin 目录下，并设置该目录到系统环境变量

#### 启动节点
注意修改ip 地址和端口

```
etcd -name e0 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:2380 \
          -listen-peer-urls http://0.0.0.0:2380 \
            -listen-client-urls http://0.0.0.0:3379 \
              -advertise-client-urls http://0.0.0.0:3379 \
                -initial-cluster e0=http://0.0.0.0:2380,e1=http://0.0.0.0:23801,e2=http://0.0.0.0:23802 \
                -initial-cluster-state new
```

```
etcd -name e1 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:23801 \
          -listen-peer-urls http://0.0.0.0:23801 \
            -listen-client-urls http://0.0.0.0:33791 \
              -advertise-client-urls http://0.0.0.0:33791 \
                -initial-cluster e0=http://0.0.0.0:2380,e1=http://0.0.0.0:23801,e2=http://0.0.0.0:23802 \
                -initial-cluster-state new
```

```
etcd -name e2 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:23802 \
          -listen-peer-urls http://0.0.0.0:23802 \
        -listen-client-urls http://0.0.0.0:33792 \
              -advertise-client-urls http://0.0.0.0:33792 \
                -initial-cluster e0=http://0.0.0.0:2380,e1=http://0.0.0.0:23801,e2=http://0.0.0.0:23802 \
                -initial-cluster-state new                                          
```

```
etcd -name e3 -initial-advertise-peer-urls http://0.0.0.0:23803 \
          -listen-peer-urls http://0.0.0.0:23803 \
            -listen-client-urls http://0.0.0.0:33793 \
              -advertise-client-urls http://0.0.0.0:33793 \
               -initial-cluster e0=http://0.0.0.0:2380,e2=http://0.0.0.0:23802,e1=http://0.0.0.0:23801,,e3=http://0.0.0.0:23803 \
                -initial-cluster-state existing
```
单节点 启动脚本

```
etcd -name e0 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://0.0.0.0:2380 -listen-peer-urls http://0.0.0.0:2380 -listen-client-urls http://0.0.0.0:3379 -advertise-client-urls http://0.0.0.0:3379 -initial-cluster e0=http://0.0.0.0:2380 -initial-cluster-state new

```

#### 设置访问权限

```
# 注意修改密码
etcdctl --endpoints 127.0.0.1:3379 user add root:test123456
etcdctl --endpoints 127.0.0.1:3379 user add reader:test123456
etcdctl --endpoints 127.0.0.1:3379 role add reader
etcdctl --endpoints 127.0.0.1:3379 role grant-permission reader read asmb/ asmb~
etcdctl --endpoints 127.0.0.1:3379 user grant reader reader
etcdctl --endpoints 127.0.0.1:3379 auth enable

```
#### 检查状态结果

```
etcdctl put asmb/1 1
etcdctl get asmb/1 1

etcdctl --endpoints 127.0.0.1:3379 --user root:test123456 put asmb/1 1
etcdctl --endpoints 127.0.0.1:3379 --user reader:test123456 get asmb/1

```


取消访问权限

```
etcdctl --user root:123456 auth disable
```

参考:https://blog.csdn.net/u013761036/article/details/103900225


