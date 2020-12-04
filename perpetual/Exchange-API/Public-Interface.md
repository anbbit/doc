# 公开接口

## 交易对列表

* 请求

        GET /api/swap/v1/public/symbol/list

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [
            {
                "symbol": "BTC_USDT_P",
                "price_decimal": 2,             // 价格精度
                "qty_decimal": 4,               // 数量精度
                "state": 2,                     // 状态
                "limit_qty_min": 0.001,         // 最小数量限制
                "limit_qty_max": 1000.0,        // 最大数量限制
                "limit_amount_min": 1000.0,     // 最小总价限制
                "limit_amount_max": 1000000.0,  // 最大总价限制
                "is_support_margin": 1,         // 是否支持杠杆交易
                "is_main": true,                // 是否为主板交易对
                "is_index": false,              // 是否为指数交易对
                "is_innovate": false            // 是否为创新校对对
            }
        ]
    }
    ```

## 获取指数价格

* 请求

        GET /api/swap/v1/public/index/{symbol}

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "index_price": 19345.9567
        }
    }
    ```
