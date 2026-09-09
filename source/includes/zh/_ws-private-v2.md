<h1 id="v2-private-ws">WEBSOCKET私有频道V2</h1>

- [私有频道V2地址](#WS_HOST_PRIVATE_V2)

<h2 id="v2-private-auth">私有频道 登录认证</h2>

订阅私有频道前需要先进行登录认证操作，只有认证过的用户才可以订阅用户的资产信息以及订单状态变化信息。

### 注意:如果未进行登录认证操作，订阅私有频道会返回错误信息

> 登录请求示例

```json
{
  "event": "login",
  "params": {
    "type": "api",
    "access_key": "de0535f81d51c998b7fbcf00f189f294",
    "access_sign": "sign",
    "access_timestamp": 14000000000
  }
}
```


> 登录成功响应示例 [状态码对照表](#WSERR)

```json
{
  "ts": 1690364037503,
  "code": 200,
  "status": "OK",
  "event": "login"
}
```


> 登录失败响应示例 [请参考通用错误说明](#error_ws_request_response_demo)



<h4 id="v2-private-login-req">登录请求参数说明</h4>


| 参数名                     | 必选  | 类型     | 说明                         |
|:------------------------|:----|:-------|----------------------------|
| params.type             | 是   | string | 认证方式: <br/>`api`:apiKey认证; |
| params.access_key       | 是   | string | [鉴权说明](#auth)              |
| params.access_sign      | 是   | string | [鉴权说明](#auth)              |
| params.access_timestamp | 是   | long   | [鉴权说明](#auth)              |
| event                   | 是   | string | 事件名 [事件列表](#events)        |



<h4 id="v2-private-login-rep">登录响应参数说明</h4>


| 参数名    | 类型     | 说明                   |
|:-------|:-------|----------------------|
| event  | string | 事件名 [事件列表](#events)  |
| status | string | 状态信息 可忽略             |
| code   | int    | 状态码 [状态码对照表](#WSERR) |
| ts     | long   | unix时间戳              |



<h2 id="v2-private-asstes">私有频道 资产</h2>


订阅用户的资产变更，有数据更新时才推送。目前只支持按照产品进行订阅。

请求和响应的参数说明请参考[通用请求参数说明](#v2-req-param)和[通用响应参数说明](#v2-rep-param)

### 订阅资产数据

> 订阅请求示例

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "assets",
  "product": "ETH_USDT",
  "zip": false
}
```

> 订阅成功响应示例

```json
{
  "biz": "exchange",
  "type": "assets",
  "product": "ETH_USDT",
  "ts": 1690357632710,
  "code": 200,
  "status": "OK",
  "event": "sub"
}
```


> 资产推送数据示例,本示例只是为了说明数据结构，实际推送数据中可能只包含一个币种的资产信息

```json

{
  "biz": "exchange",
  "type": "assets",
  "product": "ETH_USDT",
  "data":
  [
    {
      "currency": "USDT",
      "available": "999970.5238",
      "hold": "29.4762"
    },
    {
      "currency": "ETH",
      "available": "999970.5238",
      "hold": "29.4762"
    }
  ]
}

```

<aside>
资产推送数据字段说明
</aside>


| 参数名             | 类型     | 说明      |
|:----------------|:-------|---------|
| product         | string | 产品名/交易对 |
| $data.currency  | string | 币种名称    |
| $data.available | string | 可用数量    |
| $data.hold      | string | 冻结数量    |


### 请求用户单个交易对资产数据


> 请求单个交易对资产数据示例

```json
{
  "event": "req",
  "biz": "exchange",
  "type": "assets",
  "product": "BTC_USDT",
  "zip": false
}
```
> 请求单个交易对资产数据响应示例

```json
{
  "biz": "exchange",
  "type": "assets",
  "product": "BTC_USDT",
  "ts": 1690357632710,
  "code": 200,
  "status": "OK",
  "event": "req",
  "data":
  [
    {
      "currency": "USDT",
      "available": "999970.5238",
      "hold": "29.4762"
    },
    {
      "currency": "BTC",
      "available": "999970.5238",
      "hold": "29.4762"
    }
  ]
}
```

<aside>
请求资产数据字段说明
</aside>


| 参数名             | 类型     | 说明      |
|:----------------|:-------|---------|
| product         | string | 产品名/交易对 |
| ts              | long   | unix时间戳 |
| $data.currency  | string | 币种名称    |
| $data.available | string | 可用数量    |
| $data.hold      | string | 冻结数量    |

<h2 id="v2-private-orders">私有频道 订单</h2>

订阅用户订单的状态变化，有数据更新时才推送。目前支持所有交易对的订单状态变化订阅。支持异步查询用户单个交易对当前委托订单数据。

请求和响应的参数说明请参考[通用请求参数说明](#v2-req-param)和[通用响应参数说明](#v2-rep-param)

**注意：订单数据支持订阅全量交易对，一次订阅所有交易对订单时请注意`product`参数使用`all`(忽略大小写)**


**订阅单交易对订单数据**

**订阅所有交易对的订单数据**

> 订阅单交易对订单请求示例

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "orders",
  "product": "ETH_USDT",
  "zip": false
}
```
> 订阅所有交易对订单请求示例

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "orders",
  "product": "all",
  "zip": false
}
```

> 订阅单交易对订单响应成功示例

```json 
{
  "biz": "exchange",
  "type": "orders",
  "product": "ETH_USDT",
  "ts": 1690363716709,
  "code": 200,
  "status": "OK",
  "event": "sub"
}
```

> 订阅所有交易对订单响应成功示例

```json 
{
  "biz": "exchange",
  "type": "orders",
  "product": "all",
  "ts": 1690363716709,
  "code": 200,
  "status": "OK",
  "event": "sub"
}
```

> 订阅所有交易对订单数据失败响应示例 [错误码对照表](#WSERR)

```json
{
  "biz": "exchange",
  "type": "orders",
  "product": "all",
  "ts": 1690362052646,
  "code": 500,
  "status": "FAIL",
  "event": "sub"
}
```

> 订阅单交易对推送订单数据示例

```json
{
  "biz": "exchange",
  "type": "orders",
  "product": "BTC_USDT",
  "data": [
    {
      "order_id": "179577241915696235",
      "product": "BTC_USDT",
      "side": "buy",
      "price": "3.53340000",
      "size": "1.00000000",
      "filled_amount": "0.00000000",
      "funds": "1.00000000",
      "filled_size": "0.00000000",
      "type": "limit",
      "timeInForce": "GTC",
      "status": "1",
      "client_oid": ""
    }
  ]
}
```


> 订阅所有交易对推订单送数据示例

```json
{
  "biz": "exchange",
  "type": "orders",//注意此处和单交易对不同
  "product": "all",//注意此处和单交易对不同
  "data":
  [
    //....结构参考单交易对推送订单数据示例
  ]
}
```


**请求单个交易对当前委托订单数据,如果请求失败,会返回失败的响应。**

> 请求用户单个交易对当前委托订单数据示例

```json
{
  "event": "req",
  "biz": "exchange",
  "type": "orders",
  "product": "BTC_USDT",
  "zip": false
}
```


> 请求用户单个交易对当前委托订单数据成功的响应示例

```json
{
  "biz": "exchange",
  "type": "orders",
  "product": "BTC_USDT",
  "ts": 1690375715106,
  "code": 200,
  "status": "OK",
  "data":
  [
    //....结构参考单交易对推送订单数据示例
  ],
  "event": "req"
}
```

<aside>
订单数据字段说明,本说明以外的其他字段请忽略
</aside>

| 参数名称          | 参数说明                  | 类型     | 
|:--------------|:----------------------|--------|
| orders_id     | 订单编号                  | string |
| product       | 商品                    | string |
| side          | buy/sell              | string |
| price         | 每单位基础货币的价格            | string |
| size          | 买入/卖出的基础货币数量          | string |
| filled_amount | 成交数量                  | string |
| funds         | 想要使用的报价货币数量           | string |
| filled_size   | 成交金额                  | string |
| type          | limit:限价单/market:市价单/ | string |
| timeInForce   | 订单有效方式                | string |
| status        | 状态                    | string |
| client_oid    | 默认"0"，用户自定义订单号              | string |

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量



----


<h2 id="v2-private-req-param">私有频道 通用请求参数说明</h2>


| 参数名     | 必选  | 类型     | 说明                  |
|:--------|:----|:-------|---------------------|
| biz     | 是   | string | [订阅模块类型](#bizs)     |
| type    | 是   | string | [订阅业务类型](#types)    |
| product | 是   | string | 产品信息                |
| event   | 是   | string | 事件名 [事件列表](#events) |
| zip     | 是   | bool   | 是否启用gzip            |


<h2 id="v2-private-rep-param">私有频道 通用响应参数说明</h2>


| 参数名     | 类型     | 必选  | 说明      | 参考值           |
|:--------|:-------|:----|---------|---------------|
| event   | string | 是   | 请求事件    | [事件](#events) |
| biz     | string | 是   | 业务线     | [业务线](#bizs)  |
| type    | string | 是   | 业务类型    | [类型](#types)  |
| product | string | 是   | 交易对	    | BTC_USDT      |
| code    | number | 是   | 错误码     |               |
| status  | string | 是   | 错误码状态   ||
| ts      | long   | 是   | unix时间戳 |               |


<h2 id="v2-private-events">私有频道Event列表</h2>


| Event 名称 | 类型     | 说明              |
|:---------|:-------|-----------------|
| sub      | string | 订阅事件,由客户端主动发起   |
| login    | string | 登录事件，由客户端主动发起   |
| unsub    | string | 取消订阅事件,由客户端主动发起 |
| req      | string | 请求事件,由客户端主动发起   |


<h2 id="v2-private-bizs">私有频道Biz列表</h2>

| Biz 名称   | 类型     | 说明   |
|:---------|:-------|------|
| exchange | string | 现货交易 |

<h2 id="v2-private-types">私有频道Type列表</h2>

| Type 名称 | 类型     | 说明     | 是否支持全部交易对数据推送订阅 |
|:--------|:-------|--------|:----------------|
| assets  | string | 资产变更   | 否               |
| orders  | string | 订单状态变更 | 是               |
