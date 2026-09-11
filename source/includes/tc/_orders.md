# 交易和訂單

<h2 id="創建新訂單">POST  創建新訂單</h2>


只有當您的賬戶有足夠的資金才能下單。


**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

POST [HOST](#HTTP-HOST)/v1/orders


> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

> <a name="ResonpseExample">REQUEST EXAMPLE</a>

```json
{
  "product": "BTC_USDT",
  "side": "buy",
  "type": "limit",
  "time_in_force": "GTC",
  "stp": "dc",
  "price": "2.00",
  "size": "3.300000000000000000",
  "client_oid": "QZ_2020-jj"
}
```

<aside>
REQUEST PARAMETERS
</aside>

**common parameters**

`product`: 必須是一個存在的商品。例如 `BTC_USD`。產品列表可以通過 [products](#Products) 獲得。

`side`: `buy` 買入， `sell` 賣出。

`type`: 訂單類型

- `limit`: 限價訂單
- `market`: 市價訂單

`client_oid`: 可選,默認"0"，用戶自定義訂單號，用於用戶管理自己的訂單。該ID非唯一，長度不多於32個字符的字符串類型，必須由大小寫字母A-Z/a-z、數字0-9、下劃線_、中劃線- 中的元素組成，不支持以外的特殊符號

`stp`: 即 `self trade prevention`。 BGE禁止用戶與自己成交的行為。用戶可以通過`stp`選項，指定當自成交場景出現時的訂單處理策略。

- `dc`: 即 `decrease and cancel` (default)。當撮合發生相同用戶訂單匹配時，數量多的一方將被執行`decrease`指令，數量少的一方將被執行`cancel`指令。 decrease 或者
  或者cancel 的數量為兩個訂單發生自成交匹配的最小值。
- `co`: 即 `cancle oldest`, 當撮合發生相同用戶訂單匹配時，掛單較早的訂單會被執行`cancle`指令。新訂單將被繼續執行正常交易撮合流程。
- `cn`: 即 `cancle newest`, 當撮合發生相同用戶訂單匹配時，最新訂單會被執行`cancle`指令，較早的掛單依然停留在訂單簿，執行正常的交易撮合流程。
- `cb`: 即 `cancle both`,當撮合發生相同用戶訂單匹配時，發生匹配的兩個訂單，均會被執行`cancle`指令。

`time_in_force`: 可選，交易指令，目前支持 `GTC`、`FOK`。默認值為 `GTC`

**limit order parameters**

`price`: 商品價格

`size`: 買入或者賣出商品的數量

**market order parameters**

`size`: 期望交易數量。需要`side` 為 `sell`，代表以最新成交價進行賣出，期望最多賣出的商品數。

`funds`: 期望交易額度。需要`side` 為 `buy`，代表以最新成交價進行買入，期望花費的最多資產額度。

| 參數名稱 | 參數說明 | 是否必須 | 數據類型 | 
| -------- | -------- | -------- | -------- |
|product|商品，例:ETH_USD|true|string|
|side|buy/sell|true|string|
|type|limit:限價單/market:市價單|true|string|
|time_in_force|GTC|string|
|stp|自成交：dc：減少和取消（默認）co：取消最舊 cn：取消最新 cb：取消兩者|true|string|
|time_in_force|交易指令,GTC|false|string|
|funds|想要使用的報價貨幣數量|false|string|
|price|每個幣的價格|false|string|
|size|買入或賣出的數量|false|string|
|client_oid|用戶自定義訂單號|false|string|

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json

{
  "order_id":129149436110880967,
  "client_oid":"Order_ower_1233"
}

```

<aside>
RESPONSE PARAMETERS
</aside>

`order_id`:  BGE所生成的訂單號

`client_oid`:  用戶自定義訂單號

| 參數名稱 | 參數說明 | 類型 | 
| -------- | -------- | ----- | 
|order_id|BGE所生成的訂單號|string|
|client_oid|用戶自定義訂單號，默認"0"|string|


<h2 id="根據系統訂單號查詢單個訂單">GET  根據系統訂單號查詢單個訂單</h2>

獲取指定訂單信息。

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

GET [HOST](#HTTP-HOST)/v1/orders/{order_id}


<a name="order_detail"></a>

> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

| 參數名稱 | 參數說明     | 是否必須 | 數據類型 | 
| -------- | ----- | -------- | -------- | 
|order_id|訂單ID|true|string||


<a name="order_detail_demo"></a>
> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
{
  "filled_size": "0.000000000000000000",
  "filled_fees": "1.00",
  "filled_amount": "0.000000000000000000",
  "filled_average_price": "0",
  "created_at": "2021-12-14T03:19:15Z",
  "updated_at": "2022-01-04T06:57:34Z",
  "price": "2.00",
  "size": "3.300000000000000000",
  "product": "BTC_USDT",
  "order_id": "127738628653088481",
  "funds": "0.000000000000000000",
  "type": "limit",
  "time_in_force": "GTC",
  "side": "buy",
  "status": "7",
  "client_oid": "QZ_2020-jj"
}
```

<aside>
RESPONSE PARAMETERS
</aside>

`status`: 交易狀態，取值範圍0-7

- 0: 已經收到訂單
- 1: 已經提交訂單
- 2: 訂單部分成交
- 3: 訂單已完全成交
- 4: 訂單發起撤銷
- 5: 訂單已經撤銷
- 6: 訂單交易失敗
- 7: 訂單被減量

| 參數名稱 | 參數說明 | 類型 | 
| -------- | -------- | ----- |
|filled_fees|成交費用|string|
|filled_size|成交金額|string|
|filled_amount|成交數量|string|
|filled_average_price|成交均價|string|
|funds|想要使用的報價貨幣數量|string|
|order_id|訂單編號|string|
|price|每單位基礎貨幣的價格|string|
|product|商品|string|
|side|buy/sell|string|
|size|買入/賣出的基礎貨幣數量|string|
|status|狀態|string|
|type|limit:限價單/market:市價單|string|
|time_in_force|GTC|string|
|created_at|創建時間|string|
|updated_at|更新時間|string|
|client_oid|用戶自定義訂單號|string|

<h2 id="根據用戶自定義訂單號查詢單個訂單">GET  根據用戶自定義訂單號查詢單個訂單</h2>

當該自定義訂單對應多個系統訂單時，只返回最新系統訂單。

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

GET [HOST](#HTTP-HOST)/v1/orders/single/{client_oid}


<a name="order_detail"></a>


> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

| 參數名稱 | 參數說明     | 是否必須 | 數據類型 | 
| -------- | ----- | -------- | -------- | 
|client_oid|用戶自定義訂單號|true|string|

`client_oid`: 當該自定義訂單對應多個系統訂單時，只返回最新系統訂單

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
{
  "filled_size": "0.000000000000000000",
  "filled_fees": "1.00",
  "filled_amount": "0.000000000000000000",
  "filled_average_price": "0",
  "created_at": "2021-12-14T03:19:15Z",
  "updated_at": "2022-01-04T06:57:34Z",
  "price": "2.00",
  "size": "3.300000000000000000",
  "product": "BTC_USDT",
  "order_id": "127738628653088481",
  "funds": "0.000000000000000000",
  "type": "limit",
  "time_in_force": "GTC",
  "side": "buy",
  "status": "7",
  "client_oid": "QZ_2020-jj"
}
```

<aside>
RESPONSE PARAMETERS
</aside>

`status`: 交易狀態，取值範圍0-7

- 0: 已經收到訂單
- 1: 已經提交訂單
- 2: 訂單部分成交
- 3: 訂單已完全成交
- 4: 訂單發起撤銷
- 5: 訂單已經撤銷
- 6: 訂單交易失敗
- 7: 訂單被減量

| 參數名稱 | 參數說明 | 類型 | 
| -------- | -------- | ----- |
|filled_fees|成交費用|string|
|filled_size|成交金額|string|
|filled_amount|成交數量|string|
|filled_average_price|成交均價|string|
|funds|想要使用的報價貨幣數量|string|
|order_id|訂單編號|string|
|price|每單位基礎貨幣的價格|string|
|product|商品|string|
|side|buy/sell|string|
|size|買入/賣出的基礎貨幣數量|string|
|status|狀態|string|
|type|limit:限價單/market:市價單|string|
|time_in_force|GTC|string|
|created_at|創建時間|string|
|updated_at|更新時間|string|
|client_oid|用戶自定義訂單號|string|


<a name="order_detail_demo"></a>

<h2 id="獲取交易明細">GET  獲取交易明細</h2>

根據不同的查詢條件可獲得符合條件的交易明細

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

GET [HOST](#HTTP-HOST)/v1/fills

> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

<aside>
REQUEST PARAMETERS
</aside>


- `order_id` 若存在，將優先查找該訂單的成交明細，其他參數將被忽略。
- `order_id` 若不存在，將根據參數組合分頁查詢所所有交易明細。其中 `limit` 最大值為1000，超過1000條的訂單明細將無法查詢
- `order_id` 若存在，則查詢條件忽略client_oid
- `client_oid` 對應多個系統訂單時，只返回最新系統訂單的明細

| 參數名稱 | 參數說明     | 是否必須 | 數據類型 | 
| -------- | --------  | -------- | -------- | 
|after|用於分頁。將結束光標設置為after日期。 |false|integer(int64)|
|before|用於分頁。將開始光標設置為before日期|false|integer(int64)|
|limit|限制返回的結果數,默認100，最大1000|false|integer(int32)|
|product|商品id|false|string|
|order_id|訂單id|false|string|
|client_oid|用戶自定義訂單號|false|string|

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
[
  {
    "product": "BTC_USDT",
    "order_id": "12808732377541664481",
    "liquidity": "maker",
    "price": "19.560000000000000000",
    "size": "0.937686000000000000",
    "fee": "0",
    "created_at": "2022-01-04T10:17:03Z",
    "updated_at": "2022-01-04T10:17:03Z",
    "side": "sell"
  }
]
```

<aside>
RESPONSE PARAMETERS
</aside>

| 參數名稱 | 參數說明 | 類型 | 
| -------- | -------- | ----- |
|created_at|訂單創建時間|string|
|updated_at|訂單更新時間|string|
|fee|按當前填寫的金額支付的費用|string|
|liquidity|流動性:marker、taker|string|
|order_id|訂單編號|string|
|price|每單位基礎貨幣的價格|string|
|product|產品編號|string|
|side| buy/sell |string|
|size|買入/賣出的基礎貨幣數量|string|

<a name="fills_detail_demo"></a>

<h2 id="獲取當前的未完結訂單">GET  獲取當前的未完結訂單</h2>

獲取沒有成交的訂單

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

GET [HOST](#HTTP-HOST)/v1/orders


> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

此接口可查詢單個訂單詳情，或根據商品的多個訂單

- `order_id` 若存在，將優先查找該訂單詳情，其他參數將被忽略。
- `order_id` 若不存在，將根據參數組合分頁查詢所所有訂單詳情。其中 `limit` 最大值為1000，超過1000條的訂單詳情將無法查詢
- `order_id` 若存在，則查詢條件忽略client_oid

| 參數名稱 | 參數說明     | 是否必須 | 數據類型 | 
| -------- | --------  | -------- | -------- | 
|after|用於分頁。將結束光標設置為after日期。 |false|integer(int64)|
|before|用於分頁。將開始光標設置為before日期|false|integer(int64)|
|limit|限制返回的結果數,默認100，最大1000|false|integer(int32)|
|order_id|訂單id|false|string|
|client_oid|用戶自定義訂單號|false|string|
|product|商品id|false|string|

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

 ```json
[
  {
    "price": "19.560000000000000000",
    "size": "35.447206000000000000",
    "product": "BTC_USDT",
    "order_id": "128087977541664481",
    "funds": "592.668000000000000000",
    "type": "limit",
    "time_in_force": "GTC",
    "side": "sell",
    "status": "2"
  }
]
```

<aside>
RESPONSE PARAMETERS
</aside>

`status`: 交易狀態，取值範圍0-7

- 0: 已經收到訂單
- 1: 已經提交訂單
- 2: 訂單部分成交
- 3: 訂單已完全成交
- 4: 訂單發起撤銷
- 5: 訂單已經撤銷
- 6: 訂單交易失敗
- 7: 訂單被減量

| 參數名稱 | 參數說明 | 類型 |
| -------- | -------- | ----- |
|funds|想要使用的報價貨幣數量|string|
|order_id|訂單編號|string|
|price|每單位基礎貨幣的價格|string|
|product|產品編號|string|
|side|buy/sell|string|
|size|買入/賣出的基礎貨幣數量|string|
|status|狀態|string|
|type|limit:限價單/market:市價單|string|
|time_in_force|GTC|string|



<a name="order_trade_detail_demo"></a>

<h2 id="根據系統訂單號撤銷單個訂單">DELETE  根據系統訂單號撤銷單個訂單</h2>


撤銷指定的未完成訂單

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

DELETE [HOST](#HTTP-HOST)/v1/orders/{order_id}


> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

| 參數名稱 | 參數說明     | 是否必須 | 數據類型 | 
| -------- | --------  | -------- | -------- | 
|order_id|訂單id|true|string|


<a name="order_trade_detail_demo"></a>

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
"1277087538632480481"
```

<aside>
RESPONSE PARAMETERS
</aside>
| 參數名稱 | 參數說明     |  數據類型 | 
| -------- | --------  |  -------- | 
|order_id|訂單id|string|

<h2 id="根據用戶自定義訂單號撤銷單個訂單">DELETE  根據用戶自定義訂單號撤銷單個訂單</h2>


可撤銷單個訂單，若自定義訂單對應多個系統訂單，撤銷最新一個訂單，撤銷申請成功，返回對應的系統訂單號

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

DELETE [HOST](#HTTP-HOST)/v1/orders/single/{client_oid}


> 鑑權信息

> 私有信息的鑑權信息，請參考 [鑑權說明](#auth)

<aside>
REQUEST PARAMETERS
</aside>


| 參數名稱 | 參數說明     | 是否必須 | 數據類型 | 
| -------- | --------  | -------- | -------- | 
|client_oid|用戶自定義訂單號|true|string|


<a name="order_trade_detail_demo"></a>

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
"1277087538632480481"
```
<aside>
RESPONSE PARAMETERS
</aside>

即將被取消的訂單的ID列表

<h2 id="根據商品取消所有訂單">DELETE  根據商品取消所有訂單</h2>


此方法為異步方法，當用戶收到接口返回時，並不代表所有訂單已經取消成功。 BGE收到請求之後，會查詢用戶賬戶下對應商品ID的所有未成交訂單，並對這些訂單異步進行取消操作。
用戶可以通過[/orders/{order_id}](#order_detail) 查詢單個訂單的成交狀態。

**限速：10次/s**

**限速規則：ApiKey**

**HTTP請求**

DELETE [HOST](#HTTP-HOST)/v1/orders


<aside>
REQUEST PARAMETERS
</aside>

`product`: 被撤銷的商品ID,例如 "BTC_USD"

| 參數名稱 | 參數說明 |  是否必須 | 數據類型 | 
| -------- |  ----- | -------- | -------- |
|product|商品|true|string||

<aside>
RESPONSE PARAMETERS
</aside>

即將被取消的訂單的ID列表
