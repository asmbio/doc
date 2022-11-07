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
# 如果需要外网访问注意将ip修改成外网可访问的ip或者 0.0.0.0
etcd -name e0 -heartbeat-interval 1000 -election-timeout 5000 -initial-advertise-peer-urls http://127.0.0.1:2380 -listen-peer-urls http://127.0.0.1:2380 -listen-client-urls http://127.0.0.1:2379 -advertise-client-urls http://127.0.0.1:2379 -initial-cluster e0=http://127.0.0.1:2380 -initial-cluster-state new

```

#### 设置访问权限

```
# 注意修改写入权限密码，防止数据被别人修改
etcdctl   user add root:test123456
etcdctl   user add reader:test123456
etcdctl   role add reader
etcdctl   role grant-permission reader read asmb/ asmb~
etcdctl   user grant reader reader
etcdctl   auth enable

# --endpoints 

```
#### 检查状态结果

```
etcdctl put asmb/1 1
etcdctl get asmb/1 1

etcdctl   --user root:test123456 put asmb/1 1
etcdctl  --user reader:test123456 get asmb/1

```


取消访问权限

```
etcdctl --user root:123456 auth disable
```

参考:https://blog.csdn.net/u013761036/article/details/103900225


