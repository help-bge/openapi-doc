<h1 id="v2-private-ws">WEBSOCKET private channel V2</h1>

- [Private channel V2 address](#WS_HOST_PRIVATE_V2)

<h2 id="v2-private-auth">Private channel login authentication</h2>

Login authentication is required before subscribing to a private channel. Only authenticated users can subscribe to the user's asset information and order status change information.

### Note: If no login authentication operation is performed, subscribing to a private channel will return an error message

> Login request example

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


> Login success response example [status code comparison table](#WSERR)

```json
{
  "ts": 1690364037503,
  "code": 200,
  "status": "OK",
  "event": "login"
}
```


> Login failure response example [Please refer to the general error description](#error_ws_request_response_demo)



<h4 id="v2-private-login-req">Login request parameter description</h4>


| parameter name | required | type | description |
|:----------------------|:----|:-------|---------------------------|
| params.type | yes | string | authentication method: <br/>`api`:apiKey authentication; |
| params.access_key | Yes | string | [Authentication Description](#auth) |
| params.access_sign | Yes | string | [Authentication Description](#auth) |
| params.access_timestamp | yes | long | [authentication description](#auth) |
| event | yes | string | event name [event list](#events) |



<h4 id="v2-private-login-rep">Login response parameter description</h4>


| parameter name | type | description |
|:-------|:-------|----------------------|
| event | string | event name [event list](#events) |
| status | string | status information can be ignored |
| code | int | status code [status code comparison table](#WSERR) |
| ts | long | unix timestamp |



<h2 id="v2-private-assess">Private channel assets</h2>


Subscriber's asset changes will be pushed only when there is data update. Currently only supports subscription by product.

Please refer to [Common Request Parameter Description](#v2-req-param) and [General Response Parameter Description](#v2-rep-param) for request and response parameter description

### Subscribe to asset data

> Subscription request example

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "assets",
  "product": "ETH_USDT",
  "zip": false
}
```

> Subscription success response example

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


> Asset push data example, this example is just to illustrate the data structure, the actual push data may only contain asset information of one currency

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
Asset push data field description
</aside>


| parameter name | type | description |
|:----------------|:-------|---------|
| product | string | product name/trading pair |
| $data.currency | string | currency name |
| $data.available | string | available quantity |
| $data.hold | string | hold amount |


### Request the user's asset data for a single trading pair


> Request a single trading pair asset data example

```json
{
  "event": "req",
  "biz": "exchange",
  "type": "assets",
  "product": "BTC_USDT",
  "zip": false
}
```
> Request a single transaction pair asset data response example

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
Request Asset Data Field Descriptions
</aside>


| parameter name | type | description |
|:----------------|:-------|---------|
| product | string | product name/trading pair |
| ts | long | unix timestamp |
| $data.currency | string | currency name |
| $data.available | string | available quantity |
| $data.hold | string | hold amount |

<h2 id="v2-private-orders">Private Channel Orders</h2>

The status change of the subscription user's order will be pushed only when there is data update. Currently supports order status change subscription for all trading pairs. Supports asynchronous query of the user's current entrusted order data for a single trading pair.

Please refer to [Common Request Parameter Description](#v2-req-param) and [General Response Parameter Description](#v2-rep-param) for request and response parameter description

**Note: The order data supports subscribing to the full amount of trading pairs. When subscribing to orders of all trading pairs at once, please note that the `product` parameter uses `all` (ignoring case)**


**Subscription order transaction pair order data**

**Subscribe to order data for all trading pairs**

> Subscription order trading pair order request example

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "orders",
  "product": "ETH_USDT",
  "zip": false
}
```
> Subscribe to all trading pairs order request example

```json
{
  "event": "sub",
  "biz": "exchange",
  "type": "orders",
  "product": "all",
  "zip": false
}
```

> Subscription order transaction pair order response success example

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

> Subscribe to all transaction pairs order response success example

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

> Subscribe to all trading pairs order data failure response example [error code comparison table](#WSERR)

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

> Subscription order trading pair push order data example

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


> Subscribe to all trading pairs to push orders and send data examples

```json
{
  "biz": "exchange",
  "type": "orders",//Note that this is different from a single trading pair
  "product": "all",//Note that this is different from a single trading pair
  "data":
  [
    //....Structure reference single transaction pair push order data example
  ]
}
```


**Request a single transaction pair for the current entrusted order data, if the request fails, a failed response will be returned. **

> Request the user's single transaction pair current entrusted order data example

```json
{
  "event": "req",
  "biz": "exchange",
  "type": "orders",
  "product": "BTC_USDT",
  "zip": false
}
```


> An example of a successful response to requesting a single transaction from the user for the current entrusted order data

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
    //....Structure reference single transaction pair push order data example
  ],
  "event": "req"
}
```

<aside>
Description of order data fields, please ignore other fields other than this description
</aside>

| Parameter name | Parameter description | Type |
|:-------------|:----------------------|-------|
| orders_id | order ID | string |
| product | commodity | string |
| side | buy/sell | string |
| price | price per unit of base currency | string |
| size | Amount of base currency to buy/sell | string |
| filled_amount | filled amount | string |
| funds | Amount of quote currency to use | string |
| filled_size | transaction amount | string |
| type | limit: limit order/market: market order/ | string |
|time_in_force|trade command,GTC|string|
| status | status | string |
| client_oid | default "0", user-defined order number | string |

`status`: transaction status, value range 0-7

- 0: order has been received
- 1: The order has been submitted
- 2: The order is partially executed
- 3: The order has been completely filled
- 4: The order is canceled
- 5: The order has been canceled
- 6: Order transaction failed
- 7: The order is reduced



----


<h2 id="v2-private-req-param">Private channel general request parameter description</h2>


| parameter name | required | type | description |
|:----------|:----|:-------|---------------------|
| biz | is | string | [subscription module type](#bizs) |
| type | yes | string | [subscription business type](#types) |
| product | yes | string | product information |
| event | yes | string | event name [event list](#events) |
| zip | yes | bool | whether to enable gzip |


<h2 id="v2-private-rep-param">Private channel general response parameter description</h2>


| Parameter name | Type | Mandatory | Description | Reference value |
|:-------|:-------|:----|---------|--------------|
| event | string | yes | request event | [event](#events) |
| biz | string | yes | line of business | [line of business](#bizs) |
| type | string | yes | business type | [type](#types) |
| product | string | yes | trading pair | BTC_USDT |
| code | number | yes | error code | |
| status | string | yes | error code status ||
| ts | long | yes | unix timestamp | |


<h2 id="v2-private-events">Private channel event list</h2>


| Event Name | Type | Description |
|:---------|:-------|-----------------|
| sub | string | Subscribe to events, initiated by the client |
| login | string | login event, initiated by the client |
| unsub | string | unsubscribe event, initiated by the client |
| req | string | request event, initiated by the client |


<h2 id="v2-private-bizs">Private Channel Biz List</h2>

| Biz Name | Type | Description |
|:---------|:-------|------|
| exchange | string | spot transaction |

<h2 id="v2-private-types">Private Channel Type List</h2>

| Type Name | Type | Description | Whether to support data push subscription for all trading pairs |
|:--------|:--------|--------|:----------------|
| assets | string | asset changes | no |
| orders | string | order status change | yes |
