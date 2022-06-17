<!-- vscode-markdown-toc -->
* 1. [安装下载](#)
	* 1.1. [从源码编译](#-1)
	* 1.2. [命令行cli 版本 安装](#cli)
	* 1.3. [桌面版本 版本 安装](#-1)
	* 1.4. [cli 命令](#cli-1)
	* 1.5. [asmb 使用说明](#asmb)
	* 1.6. [wallet 使用说明](#wallet)
* 2. [如何启动一个本地区块链网络](#-1)
* 3. [如何启动完整观察链](#-1)
* 4. [如何启动一个生产节点](#-1)
* 5. [如何添加一个备用服务器资源](#-1)
* 6. [如何借用观察链启动独立运算钱包](#-1)
* 7. [如何借用观察链启动远程钱包](#-1)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->

# ASMB（自动伸缩网状区块链）

ASMB（自动伸缩网状区块链），采样DPOS+ 共识算法分片技术理论上实现超亿级tps，上不封顶。
单账号消息tps可达3000/s，下面是更详细的技术参数
- 多->单：输出端每个账号1000/s，输入端接收3000/s 左右，二次分片后可实现单账号无上限接收速度
- 单->多：输出端3000/s左右



##  1. <a name=''></a>安装下载
###  1.1. <a name='-1'></a>从源码编译
- 编译  go build
- //+debug 《该注释下内容逻辑无关，可以注释掉，提高性能

###  1.2. <a name='cli'></a>命令行cli 版本 安装
下载地址:[github.com/asmbio/doc/releases/](https://github.com/asmbio/doc/releases/)
1. windows 安装
    从上面地址选择相应的版本后，将下载后的文件放到一个目录下，比如C:\asmb\mywallet> , 然后打开命令行，即可执行命令，或者设置一下windows 环境变量更加方便
    ```
    ./wallet -h
    ```
   
2. Ubuntu 安装



###  1.3. <a name='-1'></a>桌面版本 版本 安装


1.  下载
3.  xxxx

###  1.4. <a name='cli-1'></a>cli 命令

- 创建一个钱包

``` 
./wallet init 
```

- 创建一个新地址
  
```
./wallet new
```
- 查看地址列表
  
```
./wallet list
```

- 删除一个地址
  
```
./wallet delete -a t1hftqqtkxjjlechclxkzxapngzmkcwpfhcwesnra
```

- 查看区块

```
wallet block getblocks -h
```

- 查看账户余额

```
wallet account -h
```

- 查看分片

```
 wallet slice  -h
```

- 转账

```
wallet Trans -h 
```


###  1.5. <a name='asmb'></a>asmb 使用说明

```
asmb -h
```


###  1.6. <a name='wallet'></a>wallet 使用说明

```
wallet -h
```

##  2. <a name='-1'></a>如何启动一个本地区块链网络

硬编码测试链参数

以下参数，相同的区块链网络要保持一致，不然会出现不同的分叉
```
const ASMB_ID = "0" // 本地测试链0, 测试链1，主链2,个人测试选择其他id，防止重复
const Chainid = 0   //区块链id 与上面相同 类型不同

//const RAFT_LVLDB_SMSGS = "asmb/smsgs/" //区块分片massagemanager,其他分片的数据
const DefaultGenesisAccountNum = 100 // 重新生成 genesis config 时使用
const Producer_n = 3                 // DOPS+ 超级节点总数量轮流出块，主网 设置为21+
const Minfork = Producer_n           // 最小连续出块（分叉，拆合平均统计周期）
const Forkcountdown = 3              // 倒计时区块数，准备新的片接口服务

const BLOCK_OUT_TIME = 20 //出块等待超时时间,对应当前生产者最长出块时间 在 BLOCK_OUT_TIME/2 左右
// Height%Producer_n == 生产者排名-1 时出块，所以创世块 以后从 排名二生产者出块

const Maxblocksize = 1024 * 2                     // 最大区块大小  正常设置值:1024  * 500 (bit byte ;1024 字节 大约是6个msg 大小
const Splitpercentage = 0.60                      // 分叉 百分比（连续出块大小平均大于 Maxblocksize*Splitpercentage 分拆成两个
const Mergepercentage = Splitpercentage / 10 * 4  // 拆合 百分比 ，拆根据情况将分片区间按照平均中位数 拆到左右分片
const Emptyypercentage = Splitpercentage / 10 * 5 // 补充区块区块大小 (必须在分叉百分比和合并百分比之间)

```

```
const MINER_CHAN_NUM = 10 // 处理消息协程数量（根据服务器性能来配置
```

配置etcd 集群
[https://gitee.com/asmb/doc/blob/master/etcd.md](https://github.com/asmbio/doc/blob/master/etcd.md)]

初始化仓库,如果你拥有所有创世生产者密钥，只需要开启一个node，如果创世者有多个，由不同的创世者分别开启,每个完成node必须维护一个单独etcd 分片管理集群
```
mkdir node1
cd node1
node1$ ./asmb init 
mkdir node2
cd node2
node2$ ./asmb init 
```
导入密钥
```
# export.w 包含加密后的密钥数据
wallet import -hkey export.w
```

拷贝修改配置文件 asmbcfg，将下面字段修改成外网可访问的ip和端口，将 FullapilistConfig 修改成自己的etcd集群

```
"Rpcclientaddr": "asmb.site:8106"
"FullapilistConfig": {
		"MinersliceAeskey": "eyJhbGciOiJIUzI1eyJhbGciOiJIUzI1", //分片管理加密秘钥，用于解析分片服务 token，拥有该秘钥才能访问etcd 注册的节点服务
		"MinersliceNet": "mountain",
		"Endpoints": [
			"asmb.site:2379"									// Endpoints
		],
		"Username": "xxx",										// 读写权限用户密码
		"Password": "xxxxxx"
	},
``` 
完整 asmbcfg 配置
```
{
	# rpc 服务相关配置
	"Rpccfg": {
		"Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBbGxvdyI6WyJyZWFkIiwid3JpdGUiLCJzaWduIiwiYWRtaW4iXX0.ae22SYWjZ_RJRRhfWDpVFzWThu_6EQ-iBgAn8vdrR-w",
		"APISecret": "123456"									// rpc jwt 秘钥
	},
	"Rpcaddr": ":8106",
	"Rpcclientaddr": "192.168.106.9:8106",
	"NodeMode": "HeavyNode",
	"LookBack": 0,
	# 本节点所在etcd 分片管理器 
	"FullapilistConfig": {
		"MinersliceAeskey": "eyJhbGciOiJIUzI1eyJhbGciOiJIUzI1", //分片管理加密秘钥，用于解析分片服务 token，拥有该秘钥才能访问etcd 注册的节点服务
		"MinersliceNet": "mountain",
		"Endpoints": [
			"asmb.site:2379"									// Endpoints
		],
		"Username": "xxx",										// 读写权限用户密码
		"Password": "xxxxxx"
	},
	# 本etcd 节点故障后，通过下面etcd 集群同步数据
	"FullapilistConfigs": [
		{
			"MinersliceAeskey": "eyJhbGciOiJIUzI1eyJhbGciOiJIUzI1",
			"MinersliceNet": "",
			"Endpoints": [
				"94.191.121.95:3379"
			],
			"Username": "reader",
			"Password": "test123456"
		}
	],
	"NormalServerLoadCount": 9,	
	"SliceMgBackupCount":1
}
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

创世旷工开启 ./asmb genesisminer

启动节点1
```
# asmb genesisminer 默认加载所有生产者，如果你导入了所有生产者密钥的话，不然会报错误，无法签名
node1$ asmb genesisminer -miners 2YK3vbyZtstMS4ajXPxZhwksZDbv -rank 2
```
启动节点2
```
node2$ asmb genesisminer -miners CkPw1fzw3tgN6t8NFRuP1hHnSw7 -miners 492xZNKN8FdH9NF3gUWX8A1iHbVj  -rank 0 -rank 1
```



##  3. <a name='-1'></a>如何启动完整观察链

##  4. <a name='-1'></a>如何启动一个生产节点
如果是创世时期，必须拥有Genesis 创世者密钥才能启动生产节点

正常生产节点在获得投票排名之前处于观察状态，排名更新后自动开始生产

##  5. <a name='-1'></a>如何添加一个备用服务器资源
配置etcd 集群
[https://gitee.com/asmb/doc/blob/master/etcd.md](https://github.com/asmbio/doc/blob/master/etcd.md)]

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

拷贝修改配置文件 asmbcfg，将下面字段修改成外网可访问的ip和端口

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

##  6. <a name='-1'></a>如何借用观察链启动独立运算钱包
##  7. <a name='-1'></a>如何借用观察链启动远程钱包

无论那种情况启动钱包wallet，秘钥都在本地加密管理，如果本地设备损坏或者丢失，秘钥也会丢失无法找回


下载地址 


