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
                "symbol": "BTC_USDT",
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

## 币种列表

* 请求

        GET /api/swap/v1/public/currency/list

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": [
            {
                "currency": "ETH",             // 币种
                "chain_name": "ETH",           // 链名
                "full_name": "ETH",            // 币种全名
                "is_memo_support": 0,          // 是否支持memo，0-不支持，1-支持
                "min_withdraw_amount": 0.0001, // 最小提币金额
                "min_deposit_amount": 0.0001,  // 最小充值金额
                "withdraw_fee": 0.0001,        // 提币费用
                "exchange_confirm": 12,        // 充值确认数
                "withdraw_confirm": 12,        // 提币确认数
                "decimal": 18,                 // 链上精度
                "is_depositable": 1,           // 是否可充值
                "is_withdrawable": 1           // 是否可提币
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
    | symbol | string | 是  | 交易对名称 |

* 响应

    ```
    {
        "status": 0,
        "msg": "ok",
        "data": {
            "symbol": "PT05_USDT",
            "index_price": 0.7899,          // 指数价格
            "timestamp_ms": 1587903200000,  // 指数价格对应的时间戳
            "reference_weight": null  // 生成指数价格的各数据源权重，当前此字段为null
        }
    }
    ```
