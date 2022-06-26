### 分片测试

1. 分叉参数 [硬编码参数](%E5%85%A5%E9%97%A8.md#区块链硬编码参数)
2. 导入测试秘钥

```
wallet import -khex export.w
```
3. min3 分叉&左+测试数据（此用例先分叉后再讲左侧合并到右侧

```
asmb --endpoints 127.0.0.1:2379 test min3
```
4. max4 分叉&右+测试数据（此用例先分叉后再将右侧合并到左侧,测试时需要注释cfmtrans

```
asmb --endpoints 127.0.0.1:2379 test max4
```
5. 34 分叉 &分叉 & 左+右+

```
asmb --endpoints 127.0.0.1:2379 test 34
```