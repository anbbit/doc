# Private API

Information of order, include query account, operate order, query order, query billing details.

## Spot Balance List

* This interface is used to obtain the balance of each currency of the spot account. The spot account is used for spot transactions.
* The balance of the currency recharge and withdrawal will be placed in the fund account by default. If you need to conduct currency transactions, you need to transfer the balance of the fund account to the spot account first.

* Request

        POST /api/v1/private/balance/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | account_type | string | YES | spot account: 'trading' |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [{
            "currency": "BTC",
            "avail": 998.9842,    // Available balance
            "frozen": 0.0,        // Frozen balance
            "balance": 998.9842   // Total balance
        }]
    }
    ```

## Spot Balance

* Request

        POST /api/v1/private/balance

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | account_type | string | YES | spot account: 'trading' |
    | currency | string | YES |  |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "currency": "BTC",
            "avail": 998.9842,    // Available balance
            "frozen": 0.0,        // Frozen balance
            "balance": 998.9842   // Total balance
        }
    }
    ```

## Create Order

* Request

        POST /api/v1/private/order/create

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | cid | string | YES | Client order ID, non-repeatable, future extension function |
    | symbol | string | YES |  |
    | side | string | YES | Trade side: 'buy', 'sell' |
    | price | float | YES |  |
    | qty | float | YES | quantity |
    | type | string | YES | 'limit', 'market' |
    | account_type | string | NO | spot account: 'trading' |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "order_id": "3867330124",
            "state": "submitted"    // Order state
        }
    }
    ```

## Order Detail

* Request

        POST /api/v1/private/order/state

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | order_id | string | YES |  |
    | symbol | string | YES |  |

* Response
    
    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "order_id":3867330124,
            "cid":3867330124,
            "symbol":"BTC_USDT",
            "qty":0.000998,
            "price":19000,
            "side":"sell",
            "type":"limit",
            "state":"submitted",
            "update_tm_ms":1607594868430,
            "create_tm_ms":1607594868430,
            "amount":18.962,
            "filled_amount":18.962,
            "executed_amount":0,
            "filled_qty":0
        }
    }
    ```

## Cancel Order

* Request

        POST /api/v1/private/order/cancel

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | order_id | string | YES |  |
    | symbol | string | YES |  |
    | account_type | string | NO | spot account: 'trading' |

* Response
    
    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "order_id": "3867330124",
            "state": "canceled"
        }
    }
    ```

## Active Orderbook

* Request

        POST /api/v1/private/order/active/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES |  |
    | side | string | YES | 'buy', 'sell' |
    | state | string | YES | Order state |
    | start_tm_ms | int | NO | Timestamp in millisecond |
    | end_tm_ms | int | NO | Timestamp in millisecond |
    | limit | int | NO |  |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [{
            "order_id":3867330124,
            "cid":3867330124,
            "symbol":"BTC_USDT",
            "qty":0.000998,
            "price":19000,
            "side":"sell",
            "type":"limit",
            "state":"submitted",
            "update_tm_ms":1607594868430,
            "create_tm_ms":1607594868430,
            "amount":18.962,
            "filled_amount":18.962,
            "executed_amount":0,
            "filled_qty":0
        }]
    }
    ```

## History Orderbook

* Request

        POST /api/v1/private/order/history/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES |  |
    | side | string | YES | 'buy', 'sell' |
    | state | string | YES | Order state |
    | start_tm_ms | int | NO | Timestamp in millisecond |
    | end_tm_ms | int | NO | Timestamp in millisecond |
    | limit | int | NO |  |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [{
            "order_id":3867330124,
            "cid":3867330124,
            "symbol":"BTC_USDT",
            "qty":0.000998,
            "price":19000,
            "side":"sell",
            "type":"limit",
            "state":"submitted",
            "update_tm_ms":1607594868430,
            "create_tm_ms":1607594868430,
            "amount":18.962,
            "filled_amount":18.962,
            "executed_amount":0,
            "filled_qty":0
        }]
    }
    ```

## Trade Record

* Request

        POST /api/v1/private/trade/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES |  |
    | start_tm_ms | int | NO | Timestamp in millisecond |
    | end_tm_ms | int | NO | Timestamp in millisecond |
    | limit | int | NO |  |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [{
            "id":501671953,
            "symbol":"BTC_USDT",
            "price":18189.92,
            "qty":0.000009,
            "amount":0.16370928,
            "is_buy_maker":0,
            "update_tm_ms":1607498588900,
            "order_id":3857278370,
            "qty_fee":0.00000002,
            "amount_fee":0,
            "amount_fee_asset":"0.00000002",
            "side":"sell"
        }]
    }
    ```
