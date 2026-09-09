<h1 id="v2-private-ws">WEBSOCKET私有頻道V2</h1>

- [私有頻道V2地址](#WS_HOST_PRIVATE_V2)

<h2 id="v2-private-auth">私有頻道 登錄認證</h2>

訂閱私有頻道前需要先進行登錄認證操作，只有認證過的用戶才可以訂閱用戶的資產信息以及訂單狀態變化信息。

### 注意:如果未進行登錄認證操作，訂閱私有頻道會返回錯誤信息

> 登錄請求示例

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


> 登錄成功響應示例 [狀態碼對照表](#WSERR)

```json
{
  "ts": 1690364037503,
  "code": 200,
  "status": "OK",
  "event": "login"
}
```


> 登錄失敗響應示例 [請參考通用錯誤說明](#error_ws_request_response_demo)



<h4 id="v2-private-login-req">登錄請求參數說明</h4>


| 參數名                     | 必選  | 類型     | 說明                         |
|:------------------------|:----|:-------|----------------------------|
| params.type             | 是   | string | 認證方式: <br/>`api`:apiKey認證; |
| params.access_key       | 是   | string | [鑑權說明](#auth)              |
| params.access_sign      | 是   | string | [鑑權說明](#auth)              |
| params.access_timestamp | 是   | long   | [鑑權說明](#auth)              |
| event                   | 是   | string | 事件名 [事件列表](#events)        |



<h4 id="v2-private-login-rep">登錄響應參數說明</h4>


| 參數名    | 類型     | 說明                   |
|:-------|:-------|----------------------|
| event  | string | 事件名 [事件列表](#events)  |
| status | string | 狀態信息 可忽略             |
| code   | int    | 狀態碼 [狀態碼對照表](#WSERR) |
| ts     | long   | unix時間戳              |



<h2 id="v2-private-asstes">私有頻道 資產</h2>


訂閱用戶的資產變更，有數據更新時才推送。目前只支持按照產品進行訂閱。

請求和響應的參數說明請參考[通用請求參數說明](#v2-req-param)和[通用響應參數說明](#v2-rep-param)

### 訂閱資產數據

> 訂閱請求示例

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "assets",
  "product": "ETH_USDT",
  "zip": false
}
```

> 訂閱成功響應示例

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


> 資產推送數據示例,本示例只是為了說明數據結構，實際推送數據中可能只包含一個幣種的資產信息

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
資產推送數據字段說明
</aside>


| 參數名             | 類型     | 說明      |
|:----------------|:-------|---------|
| product         | string | 產品名/交易對 |
| $data.currency  | string | 幣種名稱    |
| $data.available | string | 可用數量    |
| $data.hold      | string | 凍結數量    |


### 請求用戶單個交易對資產數據


> 請求單個交易對資產數據示例

```json
{
  "event": "req",
  "biz": "exchange",
  "type": "assets",
  "product": "BTC_USDT",
  "zip": false
}
```
> 請求單個交易對資產數據響應示例

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
請求資產數據字段說明
</aside>


| 參數名             | 類型     | 說明      |
|:----------------|:-------|---------|
| product         | string | 產品名/交易對 |
| ts              | long   | unix時間戳 |
| $data.currency  | string | 幣種名稱    |
| $data.available | string | 可用數量    |
| $data.hold      | string | 凍結數量    |

<h2 id="v2-private-orders">私有頻道 訂單</h2>

訂閱用戶訂單的狀態變化，有數據更新時才推送。目前支持所有交易對的訂單狀態變化訂閱。支持異步查詢用戶單個交易對當前委託訂單數據。

請求和響應的參數說明請參考[通用請求參數說明](#v2-req-param)和[通用響應參數說明](#v2-rep-param)

**注意：訂單數據支持訂閱全量交易對，一次訂閱所有交易對訂單時請注意`product`參數使用`all`(忽略大小寫)**


**訂閱單交易對訂單數據**

**訂閱所有交易對的訂單數據**

> 訂閱單交易對訂單請求示例

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "orders",
  "product": "ETH_USDT",
  "zip": false
}
```
> 訂閱所有交易對訂單請求示例

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "orders",
  "product": "all",
  "zip": false
}
```

> 訂閱單交易對訂單響應成功示例

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

> 訂閱所有交易對訂單響應成功示例

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

> 訂閱所有交易對訂單數據失敗響應示例 [錯誤碼對照表](#WSERR)

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

> 訂閱單交易對推送訂單數據示例

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


> 訂閱所有交易對推訂單送數據示例

```json
{
  "biz": "exchange",
  "type": "orders",//注意此處和單交易對不同
  "product": "all",//注意此處和單交易對不同
  "data":
  [
    //....結構參考單交易對推送訂單數據示例
  ]
}
```


**請求單個交易對當前委託訂單數據,如果請求失敗,會返回失敗的響應。 **

> 請求用戶單個交易對當前委託訂單數據示例

```json
{
  "event": "req",
  "biz": "exchange",
  "type": "orders",
  "product": "BTC_USDT",
  "zip": false
}
```


> 請求用戶單個交易對當前委託訂單數據成功的響應示例

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
    //....結構參考單交易對推送訂單數據示例
  ],
  "event": "req"
}
```

<aside>
訂單數據字段說明,本說明以外的其他字段請忽略
</aside>

| 參數名稱          | 參數說明                  | 類型     | 
|:--------------|:----------------------|--------|
| orders_id     | 訂單編號                  | string |
| product       | 商品                    | string |
| side          | buy/sell              | string |
| price         | 每單位基礎貨幣的價格            | string |
| size          | 買入/賣出的基礎貨幣數量          | string |
| filled_amount | 成交數量                  | string |
| funds         | 想要使用的報價貨幣數量           | string |
| filled_size   | 成交金額                  | string |
| type          | limit:限價單/market:市價單/ | string |
| timeInForce   | 訂單有效方式                | string |
| status        | 狀態                    | string |
| client_oid    | 默認"0"，用戶自定義訂單號              | string |

`status`: 交易狀態，取值範圍0-7

- 0: 已經收到訂單
- 1: 已經提交訂單
- 2: 訂單部分成交
- 3: 訂單已完全成交
- 4: 訂單發起撤銷
- 5: 訂單已經撤銷
- 6: 訂單交易失敗
- 7: 訂單被減量



----


<h2 id="v2-private-req-param">私有頻道 通用請求參數說明</h2>


| 參數名     | 必選  | 類型     | 說明                  |
|:--------|:----|:-------|---------------------|
| biz     | 是   | string | [訂閱模塊類型](#bizs)     |
| type    | 是   | string | [訂閱業務類型](#types)    |
| product | 是   | string | 產品信息                |
| event   | 是   | string | 事件名 [事件列表](#events) |
| zip     | 是   | bool   | 是否啟用gzip            |


<h2 id="v2-private-rep-param">私有頻道 通用響應參數說明</h2>


| 參數名     | 類型     | 必選  | 說明      | 參考值           |
|:--------|:-------|:----|---------|---------------|
| event   | string | 是   | 請求事件    | [事件](#events) |
| biz     | string | 是   | 業務線     | [業務線](#bizs)  |
| type    | string | 是   | 業務類型    | [類型](#types)  |
| product | string | 是   | 交易對	    | BTC_USDT      |
| code    | number | 是   | 錯誤碼     |               |
| status  | string | 是   | 錯誤碼狀態   ||
| ts      | long   | 是   | unix時間戳 |               |


<h2 id="v2-private-events">私有頻道Event列表</h2>


| Event 名稱 | 類型     | 說明              |
|:---------|:-------|-----------------|
| sub      | string | 訂閱事件,由客戶端主動發起   |
| login    | string | 登錄事件，由客戶端主動發起   |
| unsub    | string | 取消訂閱事件,由客戶端主動發起 |
| req      | string | 請求事件,由客戶端主動發起   |


<h2 id="v2-private-bizs">私有頻道Biz列表</h2>

| Biz 名稱   | 類型     | 說明   |
|:---------|:-------|------|
| exchange | string | 現貨交易 |

<h2 id="v2-private-types">私有頻道Type列表</h2>

| Type 名稱 | 類型     | 說明     | 是否支持全部交易對數據推送訂閱 |
|:--------|:-------|--------|:----------------|
| assets  | string | 資產變更   | 否               |
| orders  | string | 訂單狀態變更 | 是               |
