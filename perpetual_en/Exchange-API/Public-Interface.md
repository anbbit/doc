# Public API

## Symbol List

* Request

        GET /api/swap/v1/public/symbol/list

* Response

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [
            {
                "symbol": "BTC_USDT_P",
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

## Index price

* Request

        GET /api/swap/v1/public/index/{symbol}

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
            "index_price": 19345.9567
        }
    }
    ```
