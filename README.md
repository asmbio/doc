<!-- vscode-markdown-toc -->
* 1. [安装下载](#)
	* 1.1. [从源码编译](#-1)
	* 1.2. [命令行cli 版本 安装](#cli)
	* 1.3. [桌面版本 版本 安装](#-1)
	* 1.4. [cli 命令](#cli-1)
	* 1.5. [asmb 使用说明](#asmb)
	* 1.6. [wallet 使用说明](#wallet)
* 2. [如何开启初始化一个新的链](#-1)
* 3. [如何启动完整观察链](#-1)
* 4. [如何启动一个生产节点](#-1)
* 5. [如何添加一个备用服务器资源](#-1)

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

##  2. <a name='-1'></a>如何开启初始化一个新的链


初始化仓库 
```
mkdir node1
cd node1
node1$ ./asmb init 
mkdir node2
cd node2
node2$ ./asmb init 
```
修改配置文件


创建第一个创世块 
```
node1$ ./asmb genesis

node2$ asmb genesis -block xxxxxx
```

创世旷工开启 ./asmb genesisminer

启动节点1
```
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
"Rpcclientaddr": "127.0.0.1:8106"
``` 

拷贝创世配置文件 Genesis
```
cp ../asmb/Genesis Genesis
```

启动节点

```
asmb genesisminer

```

## 如何借用观察链启动独立运算钱包
## 如何借用观察链启动远程钱包

无论那种情况启动钱包wallet，秘钥都在本地加密管理，如果本地设备损坏或者丢失，秘钥也会丢失无法找回


下载地址 


