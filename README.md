
- [ASMB（自动伸缩网状区块链）](#asmb自动伸缩网状区块链)
	- [安装下载](#安装下载)
		- [从源码编译](#从源码编译)
		- [命令行cli 版本 安装](#命令行cli-版本-安装)
		- [桌面版本 版本 安装](#桌面版本-版本-安装)
		- [asmb 使用说明](#asmb-使用说明)
		- [wallet 使用说明](#wallet-使用说明)
	- [如何启动一个本地区块链网络](#如何启动一个本地区块链网络)
	- [如何启动完整观察链](#如何启动完整观察链)
	- [如何启动一个生产节点](#如何启动一个生产节点)
	- [如何添加一个备用服务器资源](#如何添加一个备用服务器资源)
	- [如何启动离线钱包](#如何启动离线钱包)
	- [如何借用观察链启动独立钱包](#如何借用观察链启动独立钱包)
	- [如何借用观察链启动远程钱包](#如何借用观察链启动远程钱包)

# ASMB（自动伸缩网状区块链）

ASMB（自动伸缩网状区块链），采样DPOS+ 共识算法分片技术理论上实现超亿级tps，上不封顶。
单账号消息tps可达3000/s，下面是更详细的技术参数
- 多->单：输出端每个账号1000/s，输入端接收3000/s 左右，二次分片后可实现单账号无上限接收速度
- 单->多：输出端3000/s左右



## 安装下载
### 从源码编译
- 编译  go build
- //+debug 《该注释下内容逻辑无关，可以注释掉，提高性能

### 命令行cli 版本 安装
下载地址:[github.com/asmbio/doc/releases/](https://github.com/asmbio/doc/releases/)

**windows 安装步骤**

  1. 下载文件最新版版 到C:/asmb/bin/ 
  2. 设置windows 环境变量 C:\asmb\bin
  3. 查看命令帮助
   ```
   wallet -h
   ```

   
**Ubuntu 安装**


   下载 
   [github.com/asmbio/doc/releases/wallet_ubuntu_v0.0.1.tar.gz](https://github.com/asmbio/doc/releases/download/v0.0.1/wallet_ubuntu_v0.0.1.tar.gz)


### 桌面版本 版本 安装


1.  下载
3.  xxxx



### asmb 使用说明

```
asmb -h
```


### wallet 使用说明

```
wallet -h
```

## 如何启动一个本地区块链网络

硬编码测试链参数
[https://github.com/asmbio/doc/blob/master/入门.md#区块链硬编码参数](https://github.com/asmbio/doc/blob/master/入门.md#区块链硬编码参数)


配置etcd 集群
[https://gitee.com/asmb/doc/blob/master/etcd.md](https://github.com/asmbio/doc/blob/master/etcd.md)

初始化仓库
```
# 如果你拥有所有创世生产者密钥，只需要开启一个node，如果创世者有多个，由不同的创世者分别开启,每个node必须维护一个单独etcd 分片管理集群
mkdir node1
cd node1
node1$ ./asmb init 
mkdir node2
cd node2
node2$ ./asmb init 
```
导入创世者密钥
```
# export.w 包含加密后的创世者密钥数据
wallet import -hkey export.w
```

拷贝修改配置文件 asmbcfg，将下面字段修改成外网可访问的ip和端口，将 FullapilistConfig 修改成自己的etcd集群

```
"Rpcclientaddr": "asmb.site:8106"
"FullapilistConfig": {
		"MinersliceAeskey": "eyJhbGciOiJIUzI1eyJhbGciOiJIUzI1", //分片管理加密秘钥，用于解析分片服务 token，拥有该秘钥才能访问etcd 注册的节点服务
		"MinersliceNet": "mountain",
		"Endpoints": [
			"127.0.0.1:2379"									// Endpoints
		],
		"Username": "xxx",										// 读写权限用户密码
		"Password": "xxxxxx"
	},
``` 


拷贝创世文件 Genesis
```
cp ../asmb/Genesis Genesis
```

创建第一个创世块 
```
node1$ ./asmb genesis

node2$ asmb genesis -block Qmcmksga4noMXmE7Wa9gShPFWW53xXyY1sVaWNcXqtWGLp
```

创世旷工开启 

启动节点1
```
# asmb genesisminer 默认加载所有生产者，如果你导入了所有生产者密钥的话，不然会报错误，无法签名
node1$ asmb genesisminer -miners 2YK3vbyZtstMS4ajXPxZhwksZDbv -rank 2
```
启动节点2
```
node2$ asmb genesisminer -miners CkPw1fzw3tgN6t8NFRuP1hHnSw7 -miners 492xZNKN8FdH9NF3gUWX8A1iHbVj  -rank 0 -rank 1
```



## 如何启动完整观察链

etcd 服务启动
```
# 参考 如何启动一个本地区块链网络
```
初始化仓库
```
mkdir node1
cd node1
node1$ ./asmb init 
```

修改配置 参考[入门.md](%E5%85%A5%E9%97%A8.md)

启动空节点

```
# 空节点数量*每个节点最大分片数量，必须大于你要观察的区块链网络的分片数量，不然会出现资源不足的情况
# 由于是观察链，不需要生产者密钥，设置一个不存在的生产者
asmb node addnode -miners ttt -rank 99999
```

按照上面步骤启动足够多的空节点

开启同步
```

# -syncpoints 是远程etcd分片服务器节点
cd node99
# 该文件下必须有配置文件 
asmb node sync -syncpoints asmb.site:2379 
```
执行成功后,空节点接收到命令会开始同步最新区块
```
2022-06-18T18:19:34.230+0800    [34mINFO[0m   asmb    chain/minerbs_sample.go:229     开始出块-4ZrjxJnU1LA5xSyrWMNuXTvSYKwt
2022-06-18T18:19:34.230+0800    [35mDEBUG[0m  asmb    chain/ptl_mergeforking.go:746   RqParentHash sucss :10557-[255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255]
等待其他节点出块......
2022-06-18T18:19:34.231+0800    [34mINFO[0m   asmb    chain/chain_handmessge.go:141   Subscribe:asmb/0/msg/4ZrjxJnU1LA5xSyrWMNuXTvSYKwt
2022-06-18T18:19:34.231+0800    [34mINFO[0m   asmb    chain/chain_handmessge.go:182   Subscribe:asmb/0/chain/4ZrjxJnU1LA5xSyrWMNuXTvSYKwt
2022-06-18T18:19:39.158+0800    [35mDEBUG[0m  asmb    chain/ptl_mergeforking.go:746   RqParentHash sucss :10558-[255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255]
2022-06-18T18:19:39.340+0800    [35mDEBUG[0m  asmb    chain/ptl_mergeforking.go:746   RqParentHash sucss :10559-[255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255]
等待其他节点出块......
2022-06-18T18:19:49.989+0800    [35mDEBUG[0m  asmb    chain/ptl_mergeforking.go:746   RqParentHash sucss :10560-[255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255]
等待其他节点出块......
2022-06-18T18:20:00.810+0800    [35mDEBUG[0m  asmb    chain/ptl_mergeforking.go:746   RqParentHash sucss :10561-[255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255]
等待其他节点出块......
2022-06-18T18:20:11.872+0800    [35mDEBUG[0m  asmb    chain/ptl_mergeforking.go:746   RqParentHash sucss :10562-[255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255 255]
等待其他节点出块......
```

查看观察链运行状态

```
asmb --endpoints 94.191.121.95:3379 block getblocklist
asmb --endpoints 94.191.121.95:3379 slice getslicelist
```

## 如何启动一个生产节点
如果是创世时期，必须拥有Genesis 创世者密钥才能启动生产节点

正常生产节点在获得投票排名之前处于观察状态，排名更新后自动开始生产

## 如何添加一个备用服务器资源
配置etcd 集群
[https://gitee.com/asmb/doc/blob/master/etcd.md](https://github.com/asmbio/doc/blob/master/etcd.md)

初始化节点文件夹
```
mkdir node1
cd node1
asmb init
mkdir lcdb
cd lcdb
wallet init
```

导入生产者秘钥 

```
wallet import -khex export.w
```

拷贝修改配置文件 asmbcfg，将下面字段修改成外网可访问的ip和端口，修改本地etcd 分片管理ip 端口

```
"Rpcclientaddr": "asmb.site:8106"
``` 

拷贝创世文件 Genesis
```
cp ../asmb/Genesis Genesis
```

启动节点

```
asmb genesisminer

```
## 如何启动离线钱包
离线钱包不可以查看账户余额，只能管理本地密钥
## 如何借用观察链启动独立钱包
独立钱包会获取最新区块信息解析最新账户状态
## 如何借用观察链启动远程钱包
远程钱包通过观察链远程请求账户状态

1. 创建本地钱包仓库
```
mkdir mywallet
cd mywallet
# 初始化钱包会创建一个默认地址
wallet init
```
2. 请求测试代币

```
# 
# 请求代币前先查看一下账户余额
wallet account getaccount -a xxx
wallet test asknil -a xxx
# 等待并查余额(30s 以上)
wallet account getaccount -a xxx
```
3. 转账
   
```
 wallet Trans -a xxx SignTrans -to xxx -blance "1 Nihil"
```
4. 转账并验证结果

```
wallet test verify -a xxx
```


无论那种情况启动钱包wallet，秘钥都在本地加密管理，如果本地设备损坏或者丢失，秘钥也会丢失无法找回


下载地址 


