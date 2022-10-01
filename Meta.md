# 元宇宙如何接入区块链
欢迎来到我的元宇宙(毛坯)
- [](#)
    - [1 如何构建协议数据模型？](#1-如何构建协议数据模型)
    - [2 如何获取最新状态数据？](#2-如何获取最新状态数据)
    - [3 如何修改最新状态数据？](#3-如何修改最新状态数据)



### 1 如何构建协议数据模型？

构建对应元宇宙3d模型的数据模型，例如1号协议：

  ```
    // 每个账号的标准状态，1号协议消息内容对应 ExInfo 
    type StateAccount struct {
        //Nonce   uint64
        trie    *trie.Trie
        worksex *WorksmsgEx
        //	rcps         map[string]Messagebs
        Balance      *big.Int
        LockedAmount *big.Int // 质押中（投票，质押）
        ExInfo       []byte   // 附加数据，不同的应用协议数据结构不同, []Exinfo rlp 序列化后数据

        Receipts []byte //确认消息列表trie key:cid, value: Sign：1 cfm：2 	exc：3 确认状态+追加信息
    }


    type ExInfo struct {
        Ptlno   int    // 协议编号 1~100000  保留向基金会申请后才能使用，100000 之后用户自己定义，不同用户可能出现协议冲突
        Objdata []byte // 不同协议，用户状态内容编码后数据
    }

    // 协议号 1，元宇宙用户画像模型（示例，待标准化
    type MetaEx struct {
        Name     *big.Int // 昵称
        Age      byte     //年龄
        Sex      byte     //性别
        Info     string   // 简介
        Model3d  []byte   //用户3d 模型，加密后实体模型序列化数据，ipfs cid 值
        Metadata []byte   // 个人元宇宙装扮摆设数据
        Ex       []byte   // 扩展数据
    }

```

### 2 如何获取最新状态数据？

- 用户3D建模数据通过区块链接口加密存储到区块链,只能自己私钥才能访问（当然也可以公开存储，然后解析 MetaEx 数据
- 用户登录系统后通过元宇宙3d建模客户端，本地解析解密后的3d 模型数据，并在本地渲染后与其他人进行互动
```
# 通过GetAccount获取账号最新状态
GetAccount(ctx context.Context, address []byte) (*StateAccount, error)      

```
### 3 如何修改最新状态数据？



方式1. 通过协议号修改整个ExInfo.Objdata
```
Pubmsg向区块链网络发送签名后的消息
Pubmsg(ctx context.Context, transmsg Messagebs) error     
type Messagebs struct {
	Msgtype

	Body   Itrans // 可以是cid ,也可以是body(signmsg)

}
// Body
type Exmsg struct {
	Msgtype Msgtype // models.Trans
	From    types.Address
	PtlNo   int
	ObjData []byte //
	Time    uint64 //时间
}
type SignExmsg struct {
	Exmsg Exmsg
	Sign  []byte
}



```


方式2. 通过智能合约修改 ExInfo.Objdata 中的某一项，这样可以节约消息的大小，从而节约带宽，以及存储空间
```
Pubmsg向区块链网络发送签名后的消息
Pubmsg(ctx context.Context, transmsg Messagebs) error     


# 智能合约编写见[]
```


方式3. 通过协议号修改整个ExInfo.Objdata,某个字段的值
```
Pubmsg向区块链网络发送签名后的消息
Pubmsg(ctx context.Context, transmsg Messagebs) error     


// Body
type Ex2msg struct {
	Msgtype   Msgtype // models.Trans
	From      types.Address
	PtlNo     int
	FieldId   string
	FeildData []byte //
	Time      uint64 //时间
}

type SignEx2msg struct {
	Ex2msg Ex2msg
	Sign  []byte
}



```