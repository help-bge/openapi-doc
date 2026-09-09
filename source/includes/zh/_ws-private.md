# ~~WEBSOCKET PRIVATE(DEPRECATED)~~

- [私有频道地址(DEPRECATED)](#WS_HOST_PRIVATE)


## Login

进行WS通道用户认证。只有认证过的通道才可以进行订阅用户的资产信息以及订单状态变化信息

### Subscribe

> request

```json
{
  "event": "login",
  "params": {
    "type": "api",
    "access-key": "de0535f81d51c998b7fbcf00f189f294",
    "access-sign": "sign",
    "access-timestamp": 14000000000
  }
}
```

#### 请求参数

|参数名|必选|类型| 说明   |
|:----    |:---|:----- |------|
| params.type | 是  |string | 认证方式: <br/>`api`:apiKey认证;`token`:token认证     |
| params.access-key     |是  |string | [鉴权说明](#auth)   |
| params.access-sign     |是  |string | [鉴权说明](#auth)    |
| params.access-timestamp     |是  |long | [鉴权说明](#auth)    |
| event     |是  |string | 事件名 [事件列表](#events)  |

> response

```json
{
    "result": "login",
    "data":
    {
        "result": true
    }
}
```

|参数名|类型|说明|
|:----    |:----- |-----   |
| result    |string | [数据频道](#channels)|
| data.result     |bool | 订阅是否成功 |

## Assets

订阅用户的资产变更。目前只支持按照产品进行订阅。订阅全量的资产变更信息正在开发中。

### Subscribe

> request

```json
{
  "event": "sub",
  "params": {
    "biz": "exchange",
    "type": "assets",
    "product": "ETH_BTC",
    "zip": false
  }
}
```

#### 请求参数

|参数名|必选|类型| 说明   |
|:----    |:---|:----- |------|
| params.biz | 是  |string | [订阅模块类型](#bizs)     |
| params.type     |是  |string | [订阅业务类型](#types)  |
| params.product     |是  |string | 产品信息   |
| event     |是  |string | 事件名 [事件列表](#events)  |
| zip     |是  |bool | 是否启用gzip |

> response

```json
{
  "channel": "subscribe",
  "biz": "exchange",
  "type": "assets",
  "product": "ETH_BTC",
  "data": {
    "result": true
  }
}
```

#### 返回参数

|参数名|类型|说明|
|:----    |:----- |-----   |
| channel    |string | [数据频道](#channels)|
| biz    |string | [业务线](#bizs) |
| type | string |  [协议类型](#types) |
| product    |string | 产品名|
| data.result     |bool | 订阅是否成功 |

> feed

```json
{
  "biz": "exchange",
  "type": "assets",
  "product": "BTC_USDT",
  "zip": false,
  "data": [
    {
      "symbol": "BTC",
      "available": 100805.8,
      "hold": 13.2
    }
  ]
}
```

#### 推送数据

|参数名|类型|说明|
|:----    |:----- |-----   |
| channel    |string | [数据频道](#channels)|
| biz    |string | [业务线](#bizs) |
| type | string |  [协议类型](#types) |
| product    |string | 产品名|
| $data.currency     |string | 币种名称 |
| $data.available     |bigdecimal | 可用数量名称 |
| $data.hold     |bigdecimal | 冻结数量币种名称 |

## Orders

订阅所有订单的状态变化

### Subscribe

> request

```json
{
  "event": "sub",
  "params": {
    "biz": "exchange",
    "type": "orders",
    "product": "all",
    "zip": false
  }
}
```

#### 请求参数

|参数名|必选|类型| 说明   |
|:----    |:---|:----- |------|
| params.biz | 是  |string | [订阅模块类型](#bizs)     |
| params.type     |是  |string | [订阅业务类型](#types)  |
| params.product     |是  |string | 产品名   |
| event     |是  |string | 事件名 [事件列表](#events)  |
| zip     |是  |bool | 是否启用gzip |

> response

```json
{
  "channel": "subscribe",
  "biz": "exchange",
  "type": "orders",
  "product": "all",
  "data": {
    "result": true
  }
}
```

#### 返回参数

|参数名|类型|说明|
|:----    |:----- |-----   |
| channel    |string | [数据频道](#channels)|
| biz    |string | [业务线](#bizs) |
| type | string |  [协议类型](#types) |
| product    |string | 产品名|
| data.result     |bool | 订阅是否成功 |

<aside>
RESPONSE PARAMETERS
</aside>

| 参数名称 | 参数说明 | 类型 | schema |
| -------- | -------- | ----- |----- | 
|order_id|订单编号|string||
|product|商品|string||
|side| buy/sell |string||
|price|每单位基础货币的价格|string||
|size|买入/卖出的基础货币数量|string||
|filled_amount|成交数量|string||
|funds|想要使用的报价货币数量|string||
|filled_size| 成交金额 |string||
|type|limit:限价单/market:市价单/|string||
|timeInForce|订单有效方式|string|GTC、IOC、FOK|
|status|状态|string||
|client_oid| 默认"0"，用户自定义订单号 | string ||


`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量



> feed

```json
{
  "biz": "exchange",
  "type": "orders",
  "data": {
    "order_id": "124645019749408967",
    "product": "BTC_USDT",
    "side": "buy",
    "price": "60000",
    "size": "1",
    "filled_amount": "0",
    "funds": "1",
    "filled_size": "0",
    "type": "limit",
    "timeInForce": "GTC",
    "status": "1",
    "client_oid": ""
  }
}
```

#### 推送数据

|参数名|类型|说明|
|:----    |:----- |-----   |
| channel    |string | [数据频道](#channels)|
| biz    |string | [业务线](#bizs) |
| type | string |  [协议类型](#types) |
| product    |string | 产品名|
| $data    |object | [订单详情](#order_detail) |

## Event 列表

<a name="events"></a>

|Event 名称|类型|说明|
|:----    |:----- |-----   |
| sub    |string | 订阅事件,由客户端主动发起|
| login    |string | 登录事件，由客户端主动发起 |

## Biz 列表

<a name="bizs"></a>

|Biz 名称|类型|说明|
|:----    |:----- |-----   |
| exchange    |string | 现货交易|

## Type 列表

<a name="types"></a>

|Type 名称|类型|说明|
|:----    |:----- |-----   |
| assets    |string | 资产变更|
| orders    |string | 订单状态变更|
