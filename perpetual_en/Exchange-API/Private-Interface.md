# Private API

Information of perpetual order, include query account, operate order, query order, query billing details.

## Query Account Information

* Request

        POST /api/swap/v1/private/account/list

* Parameters

    null.

* Response

    ```
    [
        {
            "symbol": "BTC_USDT_P", 
            "mode": "cross",    // position mode: crossed, fixed
            "account_value": 11163176.409296853,
            "avail_margin": 11161897.76919204,      // available margin
            "position_margin": 0,
            "order_margin": 0.0, 
            "frozen_margin": 1278.64010481344,
            "unrealized_pnl": 0,         // pnl: profit and loss
            "realized_pnl": 0, 
            "transfer_balance": 11161897.76919204,
            "margin_rate": null, 
            "maint_margin_rate": 0.3283, 
            "init_margin_rate": 0.3333,
            "long_leverage": 10.0, 
            "short_leverage": 10.0,
            "positions": [], 
            "unfilled_open_qty": 0.14
        }
    ]
    ```

## Query Account State

* Request

        POST /api/swap/v1/private/account/state

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```
    {
        "symbol": "BTC_USDT_P", 
        "mode": "cross", 
        "account_value": 11163176.409296853,
        "avail_margin": 11161897.76919204, 
        "position_margin": 0,  
        "order_margin": 1278.64010481344,
        "frozen_margin": 1278.64010481344, 
        "unrealized_pnl": 0,      // pnl: profit and loss
        "realized_pnl": 0, 
        "transfer_balance": 11161897.76919204, 
        "margin_rate": null, 
        "maint_margin_rate": 0.045,
        "init_margin_rate": 0.05,
        "long_leverage": 10.0, 
        "short_leverage": 10.0, 
        "positions": [],  
        "unfilled_open_qty": 0.1 
    }
    ```

## Modify Leverage

* Request

        POST /api/swap/v1/private/account/leverage/update

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES |  |
    | leverage | float | YES |  |
    | direction | string| YES | Open direction: long, short |

* Response

    ```
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // Order id
        "state": "submitted"    // Order state:
                                // pending_submit,
                                // submitted,
                                // partial_filled,
                                // partial_canceled,
                                // filled,
                                // canceled,
                                // pending_cancel,
                                // sys_canceled
    }
    ```

## All Position of Perpetual Order

* Request

        POST /api/swap/v1/private/position

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```
    [
        {
            "position_id": 243, 
            "mode": "cross",  // Position mode: crossed, fixed
            "symbol": "BTC_USDT_P", 
            "position_margin": 4317.842646366,
            "init_margin": 31721.40101578,
            "order_margin": 0.0,
            "unrealized_pnl": -274035.58369414,
            "realized_pnl": 0.0,  
            "pnl_rate": -8.638823473081134,
            "leverage": 10.0,     
            "direction": "long",  // Position direction: long, short
            "qty": 3.927579, 
            "avail_qty": 3.927579, 
            "mark_price": 10993.64938647,
            "avg_price": 80765.78731014705,
            "liquid_price": 0.0, 
            "bankrupt_price": 0.0, 
            "amount": 317214.0101578, 
            "mark_value": 43178.42646366,
            "margin_rate": null 
        }
    ]
    ```

## Create Order

* Request

        POST /api/swap/v1/private/order/create

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | qty | float | YES | quantity |
    | price | float | YES |  |
    | trade_type | string | YES |  |

* Response

    ```
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",
        "state": "submitted"  // Order state
    }
    ```

## Create Order Batch

* Request

        POST /api/swap/v1/private/order/create/batch

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | account_type | string | YES | swap, trading, spot_margin |
    | orders | list | YES | Order list |
    | cid | string | YES | User defined order id |
    | qty | float | YES | quantity |
    | price | float | YES |  |
    | trade_type| string | YES |  |
    | amount | float  | NO |  |
    | type | string | YES | limit, market |

* Request Sample

        POST /api/swap/v1/private/order/create/batch

    ```
    {
        "symbol": "BTC_USDT_P",
        "account_type": "swap",
        "orders": [
            {
                "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",
                "qty": 0.1, 
                "price": 10000,
                "trade_type": "openlong",
                "amount": "long",
                "type": "limit",
            },
        ]
    }
    ```

* Response

    ```
    {
        "result": [
            {
                "order_id": "3.BTC_USDT_P.0.3448f869a1877429",
                "state": "submitted"  // Order state
            }
        ]
    }
    ```

## Query Order State

* Request

        POST /api/swap/v1/private/order/state

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | order_id | string | YES | Order id |

* Response

    ```
    {
        "user_id": 1, 
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",
        "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",
        "symbol": "BTC_USDT_P", 
        "side": "buy",             // buy, sell
        "type": "limit",           // limit, market
        "qty": 0.1,
        "price": 10000.0,
        "state": "submitted",      // Order state
        "amount": 1000.7, 
        "filled_qty": 0.0,
        "maker_fee": 0.0,
        "taker_fee": 0.0,
        "update_tm_ms": 1596508105976.0,
        "create_tm_ms": 1596508105976.0,
        "filled_amount": 0.0 
    }
    ```

## Cancel Order

* Request

        POST /api/swap/v1/private/order/cancel

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | order_id | string | YES | Order id |

* Response

    ```
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",
        "state": "pending_cancel"  // Order state
    }
    ```

## Cancel All Order

* Request

        POST /api/swap/v1/private/order/cancel/all

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```
    {
        "state": "pending_cancel"  // Order state
    }
    ```

## Cancel Order Batch

* Request

        POST /api/swap/v1/private/order/cancel/batch

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | order_ids | list | YES | Order id list |

* Response

    ```
    {
        "result": [
            {
                "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",
                "state": "pending_cancel"    // Order state
            }
        ]
    }
    ```

## Query Active Orderbook

* Request

        POST /api/swap/v1/private/order/active/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | account_type | string | NO | swap_p |
    | order_type | string | NO | limit, market |
    | order_state | string | NO | Order state |
    | trade_type | string | NO | openlong, openshort, closeshort, closelong |
    | page | int | NO |  |
    | size | int | NO |  |
    | start_tm_ms | int | NO |  |
    | end_tm_ms | int | NO |  |

* Response

    ```
    [
        {
            "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17", 
            "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",
            "symbol": "BTC_USDT_P", 
            "trade_type": "openlong",
            "type": "limit",
            "qty": 0.1,
            "price": 10000.0, 
            "margin": 100.77049, 
            "order_margin": 100.77049, 
            "leverage": 10.0,
            "is_plan": false,
            "trigger_price": 0.0,
            "state": "submitted",           // Order state
            "amount": 1000.7,
            "filled_amount": 0.0,
            "filled_qty": 0.0,
            "filled_rate": 0.0,
            "maker_fee_rate": -0.0002,
            "taker_fee_rate": 0.0007,
            "qty_fee": 0.0,
            "amount_fee": 0.0,
            "realized_pnl": 0.0,
            "source": "api",
            "version": 1,
            "create_tm_ms": 1596508105976, 
            "update_tm_ms": 1596508105976
        },
    ]
    ```

## Query History Orderbook

* Request

        POST /api/swap/v1/private/order/history/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | account_type | string | NO | swap_p |
    | order_type | string | NO | limit, market |
    | order_state | string | NO | Order state |
    | trade_type | string | NO | openlong, openshort, closeshort, closelong |
    | page | int | NO |  |
    | size | int | NO |  |
    | start_tm_ms | int | NO |  |
    | end_tm_ms | int | NO |  |

* Response

    ```
    [
        {
            "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17", 
            "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da", 
            "symbol": "BTC_USDT_P", 
            "trade_type": "openlong", 
            "type": "limit", 
            "qty": 0.1,  
            "price": 10000.0, 
            "margin": 100.77049, 
            "order_margin": 100.77049, 
            "leverage": 10.0, 
            "is_plan": false, 
            "trigger_price": 0.0, 
            "state": "submitted",           // Order state
            "amount": 1000.7, 
            "filled_amount": 0.0, 
            "filled_qty": 0.0,  
            "filled_rate": 0.0, 
            "maker_fee_rate": -0.0002, 
            "taker_fee_rate": 0.0007, 
            "qty_fee": 0.0, 
            "amount_fee": 0.0, 
            "realized_pnl": 0.0,  
            "source": "api", 
            "version": 1, 
            "create_tm_ms": 1596508105976, 
            "update_tm_ms": 1596508105976
        },
    ]
    ```

## Query Trade List

* Request

        POST /api/swap/v1/private/trade/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | page | int | NO |  |
    | size| int | NO |  |
    | start_tm_ms | int | NO |  |
    | end_tm_ms | int | NO |  |

* Response

    ```
    {
        "total": 9215, 
        "page": 1, 
        "list": [
            {
                "symbol": "BTC_USDT_P", 
                "leverage": 10.0,  
                "trade_type": "openshort", 
                "qty": 0.001,  
                "price": 10969.5, 
                "is_maker": true, 
                "is_buy_maker": false, 
                "amount_fee": -0.0021939,  
                "qty_fee": 0.0, 
                "realized_pnl": 0.0, 
                "create_tm_ms": 1596110439268
            }
        ]
    }
    ```

## Query Balance

* Request

        POST /api/swap/v1/private/balance

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```
    {
        "symbol": "BTC_USDT_P", 
        "currency": "USDT",  
        "avail_margin": 11161897.76919204,
        "order_margin": 0.0,
        "frozen_margin": 1278.64010481,
        "balance": 11163176.40929685,
        "float": 8,                  // precision
        "long_leverage": 10.0, 
        "short_leverage": 10.0,
        "mode": "crossed",       // Position mode: crossed, fixed
        "equal_btc": 1011.45962192          // Balance equal to btc
    }
    ```

## Query All Balance

* Request

        POST /api/swap/v1/private/balance/list

* Parameters

    null

* Response

    ```
    [
        {
            "symbol": "BTC_USDT_P", 
            "currency": "USDT",
            "avail_margin": 11161897.76919204,
            "order_margin": 0.0, 
            "frozen_margin": 1278.64010481,
            "balance": 11163176.40929685,       // Total banlace
            "float": 8,               // precision
            "long_leverage": 10.0, 
            "short_leverage": 10.0, 
            "mode": 0,         // Position mode: crossed, fixed
            "equal_btc": 1011.45962192          // Balance equal to btc
        }
    ]
    ```
