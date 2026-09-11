# 交易和订单

<h2 id="创建新订单">POST  创建新订单</h2>


只有当您的账户有足够的资金才能下单。


**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

POST [HOST](#HTTP-HOST)/v1/orders


> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

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

`product`: 必须是一个存在的商品。例如 `BTC_USD`。产品列表可以通过 [products](#Products) 获得。

`side`: `buy` 买入， `sell` 卖出。

`type`: 订单类型

- `limit`: 限价订单
- `market`: 市价订单

`client_oid`: 可选,默认"0"，用户自定义订单号，用于用户管理自己的订单。该ID非唯一，长度不多于32个字符的字符串类型，必须由大小写字母A-Z/a-z、数字0-9、下划线_、中划线- 中的元素组成，不支持以外的特殊符号

`stp`: 即 `self trade prevention`。BGE禁止用户与自己成交的行为。用户可以通过`stp`选项，指定当自成交场景出现时的订单处理策略。

- `dc`: 即 `decrease and cancel` (default)。当撮合发生相同用户订单匹配时，数量多的一方将被执行`decrease`指令，数量少的一方将被执行`cancel`指令。decrease 或者
  或者cancel 的数量为两个订单发生自成交匹配的最小值。
- `co`: 即 `cancle oldest`, 当撮合发生相同用户订单匹配时，挂单较早的订单会被执行`cancle`指令。新订单将被继续执行正常交易撮合流程。
- `cn`: 即 `cancle newest`, 当撮合发生相同用户订单匹配时，最新订单会被执行`cancle`指令，较早的挂单依然停留在订单簿，执行正常的交易撮合流程。
- `cb`: 即 `cancle both`,当撮合发生相同用户订单匹配时，发生匹配的两个订单，均会被执行`cancle`指令。

`time_in_force`: 可选，交易指令，目前支持 `GTC`、`FOK`。默认值为 `GTC`

**limit order parameters**

`price`: 商品价格

`size`: 买入或者卖出商品的数量

**market order parameters**

`size`: 期望交易数量。需要`side` 为 `sell`，代表以最新成交价进行卖出，期望最多卖出的商品数。

`funds`: 期望交易额度。需要`side` 为 `buy`，代表以最新成交价进行买入，期望花费的最多资产额度。

| 参数名称 | 参数说明                                    | 是否必须 | 数据类型 | 
| -------- |-----------------------------------------| -------- | -------- |
|product| 商品，例:ETH_USD                            |true|string|
|side| buy/sell                                |true|string|
|type| limit:限价单/market:市价单                    |true|string|
|stp| 自成交：dc：减少和取消（默认）co：取消最旧 cn：取消最新 cb：取消两者 |true|string|
|time_in_force| 订单有效方式,GTC                              |false|string|
|funds| 想要使用的报价货币数量                             |false|string|
|price| 每个币的价格                                  |false|string|
|size| 买入或卖出的数量                                |false|string|
|client_oid| 用户自定义订单号                                |false|string|

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

`order_id`:  BGE所生成的订单号

`client_oid`:  用户自定义订单号

| 参数名称 | 参数说明 | 类型 | 
| -------- | -------- | ----- | 
|order_id|BGE所生成的订单号|string|
|client_oid|用户自定义订单号，默认"0"|string|


<h2 id="根据系统订单号查询单个订单">GET  根据系统订单号查询单个订单</h2>

获取指定订单信息。

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

GET [HOST](#HTTP-HOST)/v1/orders/{order_id}


<a name="order_detail"></a>

> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

| 参数名称 | 参数说明     | 是否必须 | 数据类型 | 
| -------- | ----- | -------- | -------- | 
|order_id|订单ID|true|string||


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

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量

| 参数名称 | 参数说明                 | 类型 | 
| -------- |----------------------| ----- |
|filled_fees| 成交费用                 |string|
|filled_size| 成交金额                 |string|
|filled_amount| 成交数量                 |string|
|filled_average_price| 成交均价                 |string|
|funds| 想要使用的报价货币数量          |string|
|order_id| 订单编号                 |string|
|price| 每单位基础货币的价格           |string|
|product| 商品                   |string|
|side| buy/sell             |string|
|size| 买入/卖出的基础货币数量         |string|
|status| 状态                   |string|
|type| limit:限价单/market:市价单 |string|
|time_in_force| 订单有效方式               |string|
|created_at| 创建时间                 |string|
|updated_at| 更新时间                 |string|
|client_oid| 用户自定义订单号             |string|

<h2 id="根据用户自定义订单号查询单个订单">GET  根据用户自定义订单号查询单个订单</h2>

当该自定义订单对应多个系统订单时，只返回最新系统订单。

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

GET [HOST](#HTTP-HOST)/v1/orders/single/{client_oid}


<a name="order_detail"></a>


> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

| 参数名称 | 参数说明     | 是否必须 | 数据类型 | 
| -------- | ----- | -------- | -------- | 
|client_oid|用户自定义订单号|true|string|

`client_oid`: 当该自定义订单对应多个系统订单时，只返回最新系统订单

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

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量

| 参数名称 | 参数说明                 | 类型 | 
| -------- |----------------------| ----- |
|filled_fees| 成交费用                 |string|
|filled_size| 成交金额                 |string|
|filled_amount| 成交数量                 |string|
|filled_average_price| 成交均价                 |string|
|funds| 想要使用的报价货币数量          |string|
|order_id| 订单编号                 |string|
|price| 每单位基础货币的价格           |string|
|product| 商品                   |string|
|side| buy/sell             |string|
|size| 买入/卖出的基础货币数量         |string|
|status| 状态                   |string|
|type| limit:限价单/market:市价单 |string|
|time_in_force| 订单有效方式               |string|
|created_at| 创建时间                 |string|
|updated_at| 更新时间                 |string|
|client_oid| 用户自定义订单号             |string|


<a name="order_detail_demo"></a>

<h2 id="获取交易明细">GET  获取交易明细</h2>

根据不同的查询条件可获得符合条件的交易明细

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

GET [HOST](#HTTP-HOST)/v1/fills

> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

 
- `order_id` 若存在，将优先查找该订单的成交明细，其他参数将被忽略。
- `order_id` 若不存在，将根据参数组合分页查询所所有交易明细。其中 `limit` 最大值为1000，超过1000条的订单明细将无法查询
- `order_id` 若存在，则查询条件忽略client_oid
- `client_oid` 对应多个系统订单时，只返回最新系统订单的明细

| 参数名称 | 参数说明     | 是否必须 | 数据类型 | 
| -------- | --------  | -------- | -------- | 
|after|用于分页。将结束光标设置为after日期。|false|integer(int64)|
|before|用于分页。将开始光标设置为before日期|false|integer(int64)|
|limit|限制返回的结果数,默认100，最大1000|false|integer(int32)|
|product|商品id|false|string|
|order_id|订单id|false|string|
|client_oid|用户自定义订单号|false|string|

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

| 参数名称 | 参数说明 | 类型 | 
| -------- | -------- | ----- |
|created_at|订单创建时间|string|
|updated_at|订单更新时间|string|
|fee|按当前填写的金额支付的费用|string|
|liquidity|流动性:marker、taker|string|
|order_id|订单编号|string|
|price|每单位基础货币的价格|string|
|product|产品编号|string|
|side| buy/sell |string|
|size|买入/卖出的基础货币数量|string|

<a name="fills_detail_demo"></a>

<h2 id="获取当前的未完结订单">GET  获取当前的未完结订单</h2>

获取没有成交的订单

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

GET [HOST](#HTTP-HOST)/v1/orders


> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

此接口可查询单个订单详情，或根据商品的多个订单

- `order_id` 若存在，将优先查找该订单详情，其他参数将被忽略。
- `order_id` 若不存在，将根据参数组合分页查询所所有订单详情。其中 `limit` 最大值为1000，超过1000条的订单详情将无法查询
- `order_id` 若存在，则查询条件忽略client_oid 

| 参数名称 | 参数说明     | 是否必须 | 数据类型 | 
| -------- | --------  | -------- | -------- | 
|after|用于分页。将结束光标设置为after日期。|false|integer(int64)|
|before|用于分页。将开始光标设置为before日期|false|integer(int64)|
|limit|限制返回的结果数,默认100，最大1000|false|integer(int32)|
|order_id|订单id|false|string|
|client_oid|用户自定义订单号|false|string|
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

`status`: 交易状态，取值范围0-7

- 0: 已经收到订单
- 1: 已经提交订单
- 2: 订单部分成交
- 3: 订单已完全成交
- 4: 订单发起撤销
- 5: 订单已经撤销
- 6: 订单交易失败
- 7: 订单被减量

| 参数名称 | 参数说明                 | 类型 |
| -------- |----------------------| ----- |
|funds| 想要使用的报价货币数量          |string|
|order_id| 订单编号                 |string|
|price| 每单位基础货币的价格           |string|
|product| 产品编号                 |string|
|side| buy/sell             |string|
|size| 买入/卖出的基础货币数量         |string|
|status| 状态                   |string|
|type| limit:限价单/market:市价单 |string|
|time_in_force| 订单有效方式               |string|



<a name="order_trade_detail_demo"></a>

<h2 id="根据系统订单号撤销单个订单">DELETE  根据系统订单号撤销单个订单</h2>


撤销指定的未完成订单

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

DELETE [HOST](#HTTP-HOST)/v1/orders/{order_id}


> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

<aside>
REQUEST PARAMETERS
</aside>

| 参数名称 | 参数说明     | 是否必须 | 数据类型 | 
| -------- | --------  | -------- | -------- | 
|order_id|订单id|true|string|


<a name="order_trade_detail_demo"></a>

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
"1277087538632480481"
```

<aside>
RESPONSE PARAMETERS
</aside>
| 参数名称 | 参数说明     |  数据类型 | 
| -------- | --------  |  -------- | 
|order_id|订单id|string|

<h2 id="根据用户自定义订单号撤销单个订单">DELETE  根据用户自定义订单号撤销单个订单</h2>

 
可撤销单个订单，若自定义订单对应多个系统订单，撤销最新一个订单，撤销申请成功，返回对应的系统订单号

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

DELETE [HOST](#HTTP-HOST)/v1/orders/single/{client_oid}


> 鉴权信息

> 私有信息的鉴权信息，请参考 [鉴权说明](#auth)

<aside>
REQUEST PARAMETERS
</aside>


| 参数名称 | 参数说明     | 是否必须 | 数据类型 | 
| -------- | --------  | -------- | -------- | 
|client_oid|用户自定义订单号|true|string|


<a name="order_trade_detail_demo"></a>

> <a name="ResonpseExample">RESPONSE EXAMPLE</a>

```json
"1277087538632480481"
```
<aside>
RESPONSE PARAMETERS
</aside>

即将被取消的订单的ID列表

<h2 id="根据商品取消所有订单">DELETE  根据商品取消所有订单</h2>


此方法为异步方法，当用户收到接口返回时，并不代表所有订单已经取消成功。BGE收到请求之后，会查询用户账户下对应商品ID的所有未成交订单，并对这些订单异步进行取消操作。
用户可以通过[/orders/{order_id}](#order_detail) 查询单个订单的成交状态。

**限速：10次/s**

**限速规则：ApiKey**

**HTTP请求**

DELETE [HOST](#HTTP-HOST)/v1/orders


<aside>
REQUEST PARAMETERS
</aside>

`product`: 被撤销的商品ID,例如 "BTC_USD"

| 参数名称 | 参数说明 |  是否必须 | 数据类型 | 
| -------- |  ----- | -------- | -------- |
|product|商品|true|string||

<aside>
RESPONSE PARAMETERS
</aside>

即将被取消的订单的ID列表
