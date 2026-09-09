# ~~WEBSOCKET PRIVATE(DEPRECATED)~~

- [Private channel address (DEPRECATED)](#WS_HOST_PRIVATE)


## Login

Perform WS channel user authentication. Only authenticated channels can subscribe to the user's asset information and order status change information

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

#### Request parameters

|parameter name|required|type|description|
|:---- |:---|:----- |------|
| params.type | is |string | authentication method: <br/>`api`: apiKey authentication; `token`: token authentication |
| params.access-key | is |string | [authentication description](#auth) |
| params.access-sign | is |string | [authentication description](#auth) |
| params.access-timestamp | is |long | [authentication description](#auth) |
| event |is |string | event name [event list](#events) |

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

|parameter name|type|description|
|:---- |:----- |----- |
| result |string | [Data Channels](#channels)|
| data.result |bool | Whether the subscription is successful |

## Assets

Subscribe to user's asset changes. Currently only supports subscription by product. Subscription to full asset change information is under development.

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

#### Request parameters

|parameter name|required|type|description|
|:---- |:---|:----- |------|
| params.biz | is |string | [subscription module type](#bizs) |
| params.type | is |string | [subscription business type](#types) |
| params.product | is |string | product information |
| event |is |string | event name [event list](#events) |
| zip | yes | bool | whether to enable gzip |

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

#### return parameters

|parameter name|type|description|
|:---- |:----- |----- |
| channel |string | [data channel](#channels)|
| biz |string | [Business Line](#bizs) |
| type | string | [protocol type](#types) |
| product |string | product name|
| data.result |bool | Whether the subscription is successful |

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

#### push data

|parameter name|type|description|
|:---- |:----- |----- |
| channel |string | [data channel](#channels)|
| biz |string | [Business Line](#bizs) |
| type | string | [protocol type](#types) |
| product |string | product name|
| $data.currency |string | currency name |
| $data.available | bigdecimal | available quantity name |
| $data.hold |bigdecimal | Currency name of the frozen quantity |

## Orders

Subscribe to status changes for all orders

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

#### Request parameters

|parameter name|required|type|description|
|:---- |:---|:----- |------|
| params.biz | is |string | [subscription module type](#bizs) |
| params.type | is |string | [subscription business type](#types) |
| params.product | is |string | product name |
| event |is |string | event name [event list](#events) |
| zip | yes | bool | whether to enable gzip |

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

#### return parameters

|parameter name|type|description|
|:---- |:----- |----- |
| channel |string | [data channel](#channels)|
| biz |string | [Business Line](#bizs) |
| type | string | [protocol type](#types) |
| product |string | product name|
| data.result |bool | Whether the subscription is successful |

<aside>
RESPONSE PARAMETERS
</aside>

| parameter name | parameter description | type | schema |
| -------- | -------- | ----- |----- |
|order_id|order number|string||
|product|commodity|string||
|side| buy/sell |string||
|price|price per unit of base currency|string||
|size|Amount of base currency to buy/sell|string||
|filled_amount|Amount of transaction|string||
|funds|Amount of quote currency to use |string||
|filled_size| transaction amount |string||
|type|limit: limit order/market: market order/|string||
|timeInForce|order time-in-force instruction|string|GTC, IOC, FOK|
|status|status|string||
|client_oid| default "0", user-defined order number | string ||


`status`: transaction status, value range 0-7

- 0: order has been received
- 1: The order has been submitted
- 2: The order is partially executed
- 3: The order has been completely filled
- 4: The order is canceled
- 5: The order has been canceled
- 6: Order transaction failed
- 7: The order is reduced



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

#### push data

|parameter name|type|description|
|:---- |:----- |----- |
| channel |string | [data channel](#channels)|
| biz |string | [Business Line](#bizs) |
| type | string | [protocol type](#types) |
| product |string | product name|
| $data |object | [order details](#order_detail) |

## Event list

<a name="events"></a>

|Event Name|Type|Description|
|:---- |:----- |----- |
| sub |string | Subscribe to events, initiated by the client |
| login |string | login event, initiated by the client |

## Biz list

<a name="bizs"></a>

|Biz Name|Type|Description|
|:---- |:----- |----- |
| exchange |string | spot transaction|

## List of Types

<a name="types"></a>

|Type Name|Type|Description|
|:---- |:----- |----- |
| assets |string | asset changes|
| orders |string | order status change|
