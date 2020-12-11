# 私密接口

交易相关的信息，包括账户信息，订单操作，订单查询，账单明细查询。

## 币币账号余额列表

* 此接口用于获取币币账号的每个币种的余额，币币账号用于币币交易
* 币种充值和提币的余额默认放在资金账号，如果需要进行币币交易，需要您先将资金账号的余额划转到币币账号

* 请求

        POST /api/v1/private/balance/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | account_type | string | 是  | 账号类型，币币账号=trading |

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [{
            "currency": "BTC",
            "avail": 998.9842,    // 可用余额
            "frozen": 0.0,        // 冻结余额
            "balance": 998.9842   // 总余额
        }]
    }
    ```

## 币币账号余额详情

* 请求

        POST /api/v1/private/balance

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | account_type | string | 是  | 账号类型，币币账号=trading |
    | currency | string | 是 | 币种 |

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "currency": "BTC",
            "avail": 998.9842,    // 可用余额
            "frozen": 0.0,        // 冻结余额
            "balance": 998.9842   // 总余额
        }
    }
    ```

## 订单创建

* 请求

        POST /api/v1/private/order/create

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | cid | string | 是 | 客户端订单ID，不可重复，未来扩展功能 |
    | symbol | string | 是 | 交易对名称 |
    | side | string | 是 | 交易方向，买入='buy', 卖出='sell' |
    | price | float | 是 | 价格 |
    | qty | float | 是 | 数量 |
    | type | string | 是 | 交易类型，limit-限价, market-市价 |
    | account_type | string | 否 | 默认'trading'，币币账号=trading |

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "order_id": "3867330124",
            "state": "submitted"    // 订单状态
        }
    }
    ```

## 订单详情

* 请求

        POST /api/v1/private/order/state

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | order_id | string | 是 |  |
    | symbol | string | 是 |  |

* 响应
    
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

## 订单取消

* 请求

        POST /api/v1/private/order/cancel

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | order_id | string | 是 |  |
    | symbol | string | 是 |  |
    | account_type | string | 否 | 默认'trading'，币币账号=trading |

* 响应
    
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

## 活跃订单列表

* 请求

        POST /api/v1/private/order/active/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | side | string | 是 | 订单类型：buy-买入，sell-卖出 |
    | state | string | 是 | 订单状态 |
    | start_tm_ms | int | 否 | 起始时间，毫秒时间戳 |
    | end_tm_ms | int | 否 | 截止时间，毫秒时间戳 |
    | limit | int | 否 | 请求数量，默认20，最大100，创建时间降序排列 |

* 响应

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

## 历史订单列表

* 请求

        POST /api/v1/private/order/history/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | side | string | 是 | 订单类型：buy-买入，sell-卖出 |
    | state | string | 是 | 订单状态 |
    | start_tm_ms | int | 否 | 起始时间，毫秒时间戳 |
    | end_tm_ms | int | 否 | 截止时间，毫秒时间戳 |
    | limit | int | 否 | 请求数量，默认20，最大100，创建时间降序排列 |

* 响应

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

## 成交记录

* 请求

        POST /api/v1/private/trade/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | start_tm_ms | int | 否 | 起始时间，毫秒时间戳 |
    | end_tm_ms | int | 否 | 截止时间，毫秒时间戳 |
    | limit | int | 否 | 请求数量，默认20，最大100，创建时间降序排列 |

* 响应

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
