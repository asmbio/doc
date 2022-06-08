# ASMB（自动伸缩网状区块链）

#### 介绍
ASMB（自动伸缩网状区块链），采样DPOS+ 共识算法分片技术理论上实现超亿级tps，上不封顶。
单账号消息tps可达3000/s，下面是更详细的技术参数
- 多->单：输出端每个账号1000/s，输入端接收3000/s 左右，二次分片后可实现单账号无上限接收速度
- 单->多：输出端3000/s左右



### 安装下载
#### 从源码编译
- 编译  go build
- //+debug 《该注释下内容逻辑无关，可以注释掉，提高性能

#### 命令行cli 版本 安装
下载地址:[github.com/asmbio/doc/releases/](https://github.com/asmbio/doc/releases/)
1. windows 安装
    从上面地址选择相应的版本后，将下载后的文件放到一个目录下，比如C:\asmb\mywallet> , 然后打开命令行，即可执行命令，或者设置一下windows 环境变量更加方便
    ![image](https://user-images.githubusercontent.com/9838830/172351879-d6fe0ceb-80f7-4c32-88c0-d463ac840b4e.png)
2. Ubuntu 安装



#### 桌面版本 版本 安装


1.  下载
   

   
3.  xxxx

### cli 命令列表

#### 创建一个钱包
    './wallet init'
说明：在当前目录初始化钱包,下载查看或者执行钱包相关命令时，必须切换到该目录
- 创建一个新地址
  './wallet new'
- 查看地址列表
  './wallet list'
- 删除一个地址
  './wallet delete -a t1hftqqtkxjjlechclxkzxapngzmkcwpfhcwesnra'
#### 查看区块
'wallet block getblocks -h'
#### 查看账户余额
' wallet account -h'
#### 查看分片
' wallet slice  -h'
#### 转账
'wallet Trans -h'






#### asmb 使用说明
##### 开启一个本地测试区块链仓库
1.  初始化仓库 ./asmb init 
2.  创建第一个创世块 ./asmb genesis
3.  创世旷工开启 ./asmb genesisminer
##### 开启第二个本地测试链仓库，并连接第一个节点同步数据
1.  初始化仓库 ./asmb init -r asmb2
2.  复制创世区块cid
3.  配置第一个节点id，使得第二个节点连接到第一个节点
4.  开启区块仓库 ./asmb daemon -r asmb2
#### 备忘
##### 启动进程文件hash，如何判断是否全部升级
##### 彩蛋1
1. 启动文件hash 加密mac 、时间、
2. 加密后混淆算法 混淆mac 和 时间
3. 将混淆后的hash 定时发送到网络，等待随机抽奖

#### wallet 使用说明
下载地址 