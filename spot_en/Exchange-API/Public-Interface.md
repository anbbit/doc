# Public API

## Symbol List

* Request

        GET /api/v1/public/symbol/list

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [
            {
                "symbol": "BTC_USDT",
                "price_decimal": 2,             // price precision
                "qty_decimal": 4,               // quantity precision
                "state": 2,                     // symbol state
                "limit_qty_min": 0.001,
                "limit_qty_max": 1000.0,
                "limit_amount_min": 1000.0,
                "limit_amount_max": 1000000.0,
                "is_support_margin": 1,
                "is_main": true,
                "is_index": false,
                "is_innovate": false
            }
        ]
    }
    ```

## Currency list

* Request

        GET /api/v1/public/currency/list

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [
            {
                "currency": "ETH",
                "chain_name": "ETH",
                "full_name": "ETH",
                "is_memo_support": 0,          // 0-no, 1-yes
                "min_withdraw_amount": 0.0001,
                "min_deposit_amount": 0.0001,
                "withdraw_fee": 0.0001,
                "exchange_confirm": 12,
                "withdraw_confirm": 12,
                "decimal": 18,      // precision on the chain
                "is_depositable": 1,
                "is_withdrawable": 1
            }
        ]
    }
    ```

## Index price

* Request

        GET /api/v1/public/index/{symbol}

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES  |  |

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "symbol": "PT05_USDT",
            "index_price": 0.7899,
            "timestamp_ms": 1587903200000,
            "reference_weight": null  // Currenct return null
        }
    }
    ```
