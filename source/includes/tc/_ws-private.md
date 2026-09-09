# ~~WEBSOCKET PRIVATE(DEPRECATED)~~

- [私有頻道地址(DEPRECATED)](#WS_HOST_PRIVATE)


## Login

進行WS通道用戶認證。只有認證過的通道才可以進行訂閱用戶的資產信息以及訂單狀態變化信息

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

#### 請求參數

|參數名|必選|類型| 說明   |
|:----    |:---|:----- |------|
| params.type | 是  |string | 認證方式: <br/>`api`:apiKey認證;`token`:token認證     |
| params.access-key     |是  |string | [鑑權說明](#auth)   |
| params.access-sign     |是  |string | [鑑權說明](#auth)    |
| params.access-timestamp     |是  |long | [鑑權說明](#auth)    |
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

|參數名|類型|說明|
|:----    |:----- |-----   |
| result    |string | [數據頻道](#channels)|
| data.result     |bool | 訂閱是否成功 |

## Assets

訂閱用戶的資產變更。目前只支持按照產品進行訂閱。訂閱全量的資產變更信息正在開發中。

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

#### 請求參數

|參數名|必選|類型| 說明   |
|:----    |:---|:----- |------|
| params.biz | 是  |string | [訂閱模塊類型](#bizs)     |
| params.type     |是  |string | [訂閱業務類型](#types)  |
| params.product     |是  |string | 產品信息   |
| event     |是  |string | 事件名 [事件列表](#events)  |
| zip     |是  |bool | 是否啟用gzip |

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

#### 返回參數

|參數名|類型|說明|
|:----    |:----- |-----   |
| channel    |string | [數據頻道](#channels)|
| biz    |string | [業務線](#bizs) |
| type | string |  [協議類型](#types) |
| product    |string | 產品名|
| data.result     |bool | 訂閱是否成功 |

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

#### 推送數據

|參數名|類型|說明|
|:----    |:----- |-----   |
| channel    |string | [數據頻道](#channels)|
| biz    |string | [業務線](#bizs) |
| type | string |  [協議類型](#types) |
| product    |string | 產品名|
| $data.currency     |string | 幣種名稱 |
| $data.available     |bigdecimal | 可用數量名稱 |
| $data.hold     |bigdecimal | 凍結數量幣種名稱 |

## Orders

訂閱所有訂單的狀態變化

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

#### 請求參數

|參數名|必選|類型| 說明   |
|:----    |:---|:----- |------|
| params.biz | 是  |string | [訂閱模塊類型](#bizs)     |
| params.type     |是  |string | [訂閱業務類型](#types)  |
| params.product     |是  |string | 產品名   |
| event     |是  |string | 事件名 [事件列表](#events)  |
| zip     |是  |bool | 是否啟用gzip |

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

#### 返回參數

|參數名|類型|說明|
|:----    |:----- |-----   |
| channel    |string | [數據頻道](#channels)|
| biz    |string | [業務線](#bizs) |
| type | string |  [協議類型](#types) |
| product    |string | 產品名|
| data.result     |bool | 訂閱是否成功 |

<aside>
RESPONSE PARAMETERS
</aside>

| 參數名稱 | 參數說明 | 類型 | schema |
| -------- | -------- | ----- |----- | 
|order_id|訂單編號|string||
|product|商品|string||
|side| buy/sell |string||
|price|每單位基礎貨幣的價格|string||
|size|買入/賣出的基礎貨幣數量|string||
|filled_amount|成交數量|string||
|funds|想要使用的報價貨幣數量|string||
|filled_size| 成交金額 |string||
|type|limit:限價單/market:市價單/|string||
|timeInForce|訂單有效方式|string|GTC、IOC、FOK|
|status|狀態|string||
|client_oid| 默認"0"，用戶自定義訂單號 | string ||


`status`: 交易狀態，取值範圍0-7

- 0: 已經收到訂單
- 1: 已經提交訂單
- 2: 訂單部分成交
- 3: 訂單已完全成交
- 4: 訂單發起撤銷
- 5: 訂單已經撤銷
- 6: 訂單交易失敗
- 7: 訂單被減量



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

#### 推送數據

|參數名|類型|說明|
|:----    |:----- |-----   |
| channel    |string | [數據頻道](#channels)|
| biz    |string | [業務線](#bizs) |
| type | string |  [協議類型](#types) |
| product    |string | 產品名|
| $data    |object | [訂單詳情](#order_detail) |

## Event 列表

<a name="events"></a>

|Event 名稱|類型|說明|
|:----    |:----- |-----   |
| sub    |string | 訂閱事件,由客戶端主動發起|
| login    |string | 登錄事件，由客戶端主動發起 |

## Biz 列表

<a name="bizs"></a>

|Biz 名稱|類型|說明|
|:----    |:----- |-----   |
| exchange    |string | 現貨交易|

## Type 列表

<a name="types"></a>

|Type 名稱|類型|說明|
|:----    |:----- |-----   |
| assets    |string | 資產變更|
| orders    |string | 訂單狀態變更|
