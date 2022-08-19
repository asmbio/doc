- [ASMB API](#asmb-api)
    - [简介](#简介)
    - [接口列表](#接口列表)
    - [接口实现](#接口实现)
    - [基于接口开发的app（Maons](#基于接口开发的appmaons)

# ASMB API

### 简介
接口是基于"github.com/filecoin-project/go-jsonrpc" 开发的，符合rpc2.0 规范，目前已经实现的client有Go，C#两种。
    
### 接口列表
    
	Getchaininfo(ctx context.Context) (*Chaininfo, error)                                                                                     //perm:read
	GetLastblock(ctx context.Context, h uint64, slice []byte) (*SignBlockHeader, error)                                                       //perm:read
	GetLastblockLR(ctx context.Context, h uint64, lr string) (*SignBlockHeader, error)                                                        //perm:read
	GetPengdingMsgs(ctx context.Context, n int) ([]Messagebs, error)                                                                          //perm:read
	VdHash(ctx context.Context, hash []byte, slice1 []byte) (*SignBlockHeader, error)                                                         //perm:sign
	GetAccount(ctx context.Context, address []byte) (*StateAccount, error)                                                                    //perm:sign
	GetReceipt(ctx context.Context, addr []byte, rcpkey []byte, t uint64) ([]byte, error)                                                     //perm:sign
	GetReceipts(ctx context.Context, addr []byte, t uint64, n int) ([]Messagebs, error)                                                       //perm:sign
	GetAccounts(ctx context.Context, addr []byte, n int) (map[string]*StateAccount, error)                                                    //perm:sign
	GetAccountsbyft(ctx context.Context, hash, addrf, addrt []byte, n int) (map[string]*StateAccount, error)                                  //perm:sign
	Pubmsg(ctx context.Context, transmsg Messagebs) error                                                                                     //perm:sign
	VdMsgRequest(ctx context.Context, msgbs Messagebs) error                                                                                  //perm:sign
	GetBlockbyHS(ctx context.Context, h uint64, s []byte) (*SignBlockHeader, error)                                                           //perm:sign
	GetBlocks(ctx context.Context, h uint64, hash, s []byte, n int) ([]*SignBlockHeader, error)                                               //perm:sign	
	AskNil(ctx context.Context, addr []byte) error                                                                                            //perm:sign
	EnEgg1Code(ctx context.Context) (Egg1msg, error)                                                                                          //perm:read
	DeEgg1Code(ctx context.Context, Randomcode []byte, Time uint64) ([]byte, error)                                                           //perm:read


### 接口实现

接口实现分两部分：

第一部分，根据地址空间查找分片的服务地址（才能获得最新的数据）；

第二部分，调用查找到服务地址开放的api

参考C#实现NASMB: https://github.com/asmbio/NASMB

```
  var aRpcClient = Fullapi.FindSliceApiService(AConst.MaxSlice);

  var ret = await aRpcClient.SendRequestAsync<object>("GetBlockbyHS", null, 1, AConst.MaxSlice);
```
### 基于接口开发的app（Maons

目前Maons就是基于NASMB 开发app，目前Maons需要依托一个完整观察链，后面Maons也会作为一个独立观察节点，只观察钱包地址相关的分片数据,独立分析验证最新区块,并展示给用户.
