---
title: SFT

language_tabs:
  - ANY

search: true
---

# SFT

SFT-Solutions for tomorrow

SFT是主侧链机制的公链生态体系。主链主要提供数据存储以及调用功能，侧链则为较为复杂的合约运行提供支持。

SFT同样具备图灵完备以及以太虚拟机，为了方便合约开发者进行迁移，SFT同样支持Solidy语言进行合约编写以及跨链服务

SFT白皮书: [SFT白皮书](http://sftgroup.org/SFT_solutions_for_tomorrow.pdf "SFT白皮书")

_0基础一二三步接入区\_块\_链主网_

# 服务 Server

## 获取地址

>


```
```


> 请求返回示例

```javascript
{
  "code" : 200, # int
  "msg"  : "successed", # string
  "err"  : "null", # string
  "data" :
    {
      "address":"...ADDRESS...", #string
    },
}
```

### http请求

`POST  路径参考: "\<WEBSITE/IP\>:\<PORT\>/api/get_NewAddress/"`

### 请求参数

|名称       |描述     |类型     |必填 |样例         |取值范围|
|:----      |:--:     |:---:   |:--: |:--         |:-- |
|timestamp  |时间戳   |String  |Yes  |"1234554321" |---|
|user_id    |用户名   |String  |Yes  |"2020019428" |---|
|block      |主链名   |String  |Yes  |"Bitcoin"    |"Bitcoin"<br>"Ethereum"  |
|type       |类型     |String  |Yes  |"full"       |"full"|
|\_sign     |签名     |String  |Yes  |"mySign"     |---|

## 转账

>

```
```


> 请求返回示例

```javascript
{
  "code" : 200, # int
  "msg"  : "successed", # string
  "err"  : "null", # string
  "data" : "null", # string
}
```

### http请求

`POST  路径参考: "\<WEBSITE/IP\>:\<PORT\>/api/transfer_Out/"`

### 请求参数

|名称       |描述    |类型    |必填 |样例           |取值范围|
|:----      |:--:   |:---:   |:--: |:--           |:-- |
|timestamp  |时间戳  |String |Yes  |"1234554321"   |---|
|user_id    |用户名  |String |Yes  |"2020019428"   |---|
|block      |主链名  |String |Yes  |"Bitcoin"      |"Bitcoin"<br>"Ethereum"|
|coin_id    |货币名  |String |Yes  |"BTC"          |"BTC"<br>"ETHER"<br>"USDT"|
|order_id   |订单号  |String |Yes  |"..orderid.."  |---|
|type       |类型    |String |Yes  |"full"         |"full"|
|from       |发送者  |String |Yes  |"...from..."   |---|
|to         |接收者  |String |Yes  |"...to..."     |---|
|amount     |数额    |String |Yes  |"100.0"        |---|
|\_sign     |签名    |String |Yes  |"mySign"       |---|

# 回调 Callback

## 通知

>


```
```


> 请求返回示例

```javascript
 {
  "code" : 200, # int
  "msg"  : "successed", # string
  "err"  : "null", # string
  "data" : "null", # string
}
```

### http请求

`POST  路径参考: "\<WEBSITE/IP\>:\<PORT\>/api/notification/"`

### 请求参数

|名称       |描述   |类型   |必填  |样例         |取值范围|
|:----      |:--:  |:---:  |:--:  |:--         |:-- |
|timestamp  |时间戳 |String |Yes  |"1234554321" |---|
|status     |状态   |String |Yes  |"pending"    |"pending"<br>"confirmed"<br>"secured"<br>"cancelled"|
|block      |主链名 |String |Yes  |"Bitcoin"    |"Bitcoin"<br>"Ethereum"|
|coin_id    |货币名 |String |Yes  |"BTC"        |"BTC"<br>"ETHER"<br>"USDT"|
|app_id     |应用名 |String |Yes  |"Test_APP"   |(_用户自定义_)|
|order_id   |订单号 |String |Yes  |"..orderid.."|---|
|hash       |哈希值 |String |Yes  |"...hash..." |---|
|type       |类型   |String |Yes  |"Tx_In"      |"Tx_In"<br>"Tx_Out"|
|from       |发送者 |String |No   |"...from..." |---|
|to         |接收者 |String |Yes  |"...to..."   |---|
|amount     |数额   |String |Yes  |"100.0"      |---|
|\_sign     |签名   |String |Yes  |"mySign"     |---|
