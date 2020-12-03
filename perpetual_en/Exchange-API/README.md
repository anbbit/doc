# Perpetual Exchange API

Market infomation of perpetual order, include query account, operate order, query order, query billing details.

## Query Account Infomation

* Request

        POST /api/swap/v1/private/account/list

* Parameters

    Null.

* Response

    ```json
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
            "margin_rate": None, 
            "maint_margin_rate": 0.3283, 
            "init_margin_rate": 0.3333,
            "long_leverage": 10.0, 
            "short_leverage": 10.0,
            "positions": [], 
            "unfilled_open_qty": 0.14
        }
    ]
    ```

## 获取当前账户状态

* Request

        POST /api/swap/v1/private/account/state

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```json
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
        "margin_rate": None, 
        "maint_margin_rate": 0.045,
        "init_margin_rate": 0.05,
        "long_leverage": 10.0, 
        "short_leverage": 10.0, 
        "positions": [],  
        "unfilled_open_qty": 0.1 
    }
    ```

## 杠杆倍率修改

* Request

        POST /api/swap/v1/private/account/leverage/update

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol，如：BTC_USDT_P |
    | leverage | float | YES | 杠杆倍率 |
    | direction | string| YES | 开仓方向，long-做多，short-做空 |

* Response

    ```json
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // Order id
        "state": "submitted"    // Order state：
                                // pending_submit-提交中，
                                // submitted-已提交，
                                // partial_filled-部分成交，
                                // partial_canceled-部分撤销，
                                // filled-完全成交，
                                // canceled-已撤销，
                                // pending_cancel-撤销中，
                                // sys_canceled-系统撤销
    }
    ```

## 所有合约持仓信息

* Request

        POST /api/swap/v1/private/position

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```json
    [
        {
            "position_id": 243, 
            "mode": "cross",  // 仓位模式，crossed-全仓，fixed-逐仓
            "symbol": "BTC_USDT_P", 
            "position_margin": 4317.842646366,   // 持仓保证金
            "init_margin": 31721.40101578,       // 初始保证金
            "order_margin": 0.0,                 // 挂单冻结保证金
            "unrealized_pnl": -274035.58369414,  // 未实现盈亏
            "realized_pnl": 0.0,                 // 已实现盈亏
            "pnl_rate": -8.638823473081134,      // 盈亏率
            "leverage": 10.0,                    // 杠杆倍率
            "direction": "long",    // 持仓方向，long-做多，short-做空
            "qty": 3.927579,                     // 持仓数量
            "avail_qty": 3.927579,               // 可用数量
            "mark_price": 10993.64938647,        // 标记价格
            "avg_price": 80765.78731014705,      // 持仓均价
            "liquid_price": 0.0,                 // 强平价格
            "bankrupt_price": 0.0,               // 破产价格
            "amount": 317214.0101578,            // 开仓金额
            "mark_value": 43178.42646366,        // 持仓价值
            "margin_rate": None                  // 保证金率
        }
    ]
    ```

## 下单

创建新的合约订单。

* Request

        POST /api/swap/v1/private/order/create

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | qty | float | YES | 下单的数量 |
    | price | float | YES | 下单的委托价格 |
    | trade_type | string | YES | 下单的交易类型 |

* Response

    ```json
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // Order id
        "state": "submitted"  // Order state
    }
    ```

## 批量下单

批量创建合约订单。

* Request

        POST /api/swap/v1/private/order/create/batch

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | account_type | string | YES | 账户类型：<br>swap-合约账户，<br>trading-币币账户，<br>spot_margin-杠杆账户 |
    | orders | list | YES | 订单列表 |
    | cid | string | YES | 用户自定义Order id |
    | qty | float | YES | 下单数量 |
    | price | float | YES | 下单时的委托价格 |
    | trade_type| string | YES | 下单的订单类型 |
    | amount | float  | NO | 订单金额 |
    | type | string | YES | 订单类型：limit-限价，market-市价 |

* Request Sample

        POST /api/swap/v1/private/order/create/batch

    ```json
    {
        "symbol": "BTC_USDT_P",
        "account_type": "swap",
        "orders": [
            {
                "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",  // 用户自定义Order id
                "qty": 0.1,                // 下单数量
                "price": 10000,            // 委托价格
                "trade_type": "openlong",  // 交易类型
                "amount": "long",          // 订单金额
                "type": "limit",           // 订单类型
            },
        ]
    }
    ```

* Response

    ```json
    {
        "result": [
            {
                "order_id": "3.BTC_USDT_P.0.3448f869a1877429", // Order id
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

    ```json
    {
        "user_id": 1,                     // 用户id
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // Order id
        "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",  // 用户自定义Order id
        "symbol": "BTC_USDT_P", 
        "side": "buy",             // 订单方向：buy-买单，sell-卖单
        "type": "limit",           // 订单类型
        "qty": 0.1,                // 订单数量
        "price": 10000.0,          // 委托价格
        "state": "submitted",      // Order state
        "amount": 1000.7,          // 订单金额
        "filled_qty": 0.0,         // 完全成交量
        "maker_fee": 0.0,          // 挂单费
        "taker_fee": 0.0,          // 吃单费
        "update_tm_ms": 1596508105976.0,  // 订单更新时间
        "create_tm_ms": 1596508105976.0,  // 订单创建时间
        "filled_amount": 0.0       // 完全成交额
    }
    ```

## 撤单

* Request

        POST /api/swap/v1/private/order/cancel

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | order_id | string | YES | Order id |

* Response

    ```json
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // Order id
        "state": "pending_cancel"  // Order state
    }
    ```

## 撤掉所有订单

* Request

        POST /api/swap/v1/private/order/cancel/all

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```json
    {
        "state": "pending_cancel"  // Order state
    }
    ```

## 批量撤单

* Request

        POST /api/swap/v1/private/order/cancel/batch

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | order_ids | list | YES | Order id list |

* Response

    ```json
    {
        "result": [
            {
                "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17", // Order id
                "state": "pending_cancel"    // Order state
            }
        ]
    }
    ```

## 获取当前委托订单

* Request

        POST /api/swap/v1/private/order/active/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | account_type | string | NO | 账户类型：swap_p-交割合约 |
    | order_type | string | NO | 订单类型：limit-限价单，market-市价单 |
    | order_state | string | NO | Order state |
    | trade_type | string | NO | 交易类型：<br>openlong-买入开多，<br>openshort-卖出开空，<br>closeshort-买入平空，<br>closelong-卖出平多 |
    | page | int | NO | 页数 |
    | size | int | NO | 每页显示的条目 |
    | start_tm_ms | int | NO | 查询起始时间 |
    | end_tm_ms | int | NO | 查询结束时间 |

* Response

    ```json
    [
        {
            "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17", 
            "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",
            "symbol": "BTC_USDT_P", 
            "trade_type": "openlong",       // 交易类型
            "type": "limit",                // 报价类型
            "qty": 0.1,                     // 划转数量
            "price": 10000.0,               // 委托价格
            "margin": 100.77049,            // 保证金
            "order_margin": 100.77049,      // 订单保证金
            "leverage": 10.0,               // 杠杆倍率
            "is_plan": False,               // 是否为计划委托
            "trigger_price": 0.0,           // 触发价格
            "state": "submitted",           // Order state
            "amount": 1000.7,               // 划转金额
            "filled_amount": 0.0,           // 成交金额
            "filled_qty": 0.0,              // 成交数量
            "filled_rate": 0.0,             // 成交率
            "maker_fee_rate": -0.0002,      // 挂单费率
            "taker_fee_rate": 0.0007,       // 吃单费率
            "qty_fee": 0.0,                 // 划转数量费
            "amount_fee": 0.0,              // 划转金额费
            "realized_pnl": 0.0,            // 已实现盈亏
            "source": "api",                // 下单来源方式
            "version": 1,                   // api版本号
            "create_tm_ms": 1596508105976, 
            "update_tm_ms": 1596508105976
        },
    ]
    ```

## 获取历史委托订单

* Request

        POST /api/swap/v1/private/order/history/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | account_type | string | NO | 账户类型，如：swap_p（交割合约）|
    | order_type | string | NO | 订单类型，如：limit（限价单）、market（市价单）|
    | order_state | string | NO | Order state |
    | trade_type | string | NO | 交易类型：<br>openlong-买入开多、<br>openshort-卖出开空、<br>closeshort-买入平空、<br>closelong-卖出平多 |
    | page | int | NO | 页数 |
    | size | int | NO | 每页显示的条目 |
    | start_tm_ms | int | NO | 查询起始时间 |
    | end_tm_ms | int | NO | 查询结束时间 |

* Response

    ```json
    [
        {
            "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17", 
            "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da", 
            "symbol": "BTC_USDT_P", 
            "trade_type": "openlong",       // 交易类型
            "type": "limit",                // 报价类型
            "qty": 0.1,                     // 划转数量
            "price": 10000.0,               // 委托价格
            "margin": 100.77049,            // 保证金
            "order_margin": 100.77049,      // 订单保证金
            "leverage": 10.0,               // 杠杆倍率
            "is_plan": False,               // 是否为计划委托
            "trigger_price": 0.0,           // 触发价格
            "state": "submitted",           // Order state
            "amount": 1000.7,               // 划转金额
            "filled_amount": 0.0,           // 成交金额
            "filled_qty": 0.0,              // 成交数量
            "filled_rate": 0.0,             // 成交率
            "maker_fee_rate": -0.0002,      // 挂单费率
            "taker_fee_rate": 0.0007,       // 吃单费率
            "qty_fee": 0.0,                 // 划转数量费
            "amount_fee": 0.0,              // 划转金额费
            "realized_pnl": 0.0,            // 已实现盈亏
            "source": "api",                // 下单来源方式
            "version": 1,                   // api版本号
            "create_tm_ms": 1596508105976, 
            "update_tm_ms": 1596508105976
        },
    ]
    ```

## 获取交易列表

* Request

        POST /api/swap/v1/private/trade/list

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |
    | page | int | NO | 页数 |
    | size| int | NO | 每页显示的数量 |
    | start_tm_ms | int | NO | 查询起始时间 |
    | end_tm_ms | int | NO | 查询结束时间 |

* Response

    ```json
    {
        "total": 9215,    // 查询结果总条数
        "page": 1,        // 当前页
        "list": [
            {
                "symbol": "BTC_USDT_P", 
                "leverage": 10.0,           // 杠杆倍数
                "trade_type": "openshort",  // 交易类型
                "qty": 0.001,               // 划转数量
                "price": 10969.5,           // 委托价格
                "is_maker": True,           // 是否挂单
                "is_buy_maker": False,      // 是否买入挂单
                "amount_fee": -0.0021939,   // 划转金额手续费
                "qty_fee": 0.0,             // 划转数量手续费
                "realized_pnl": 0.0,        // 实现盈亏
                "create_tm_ms": 1596110439268
            }
        ]
    }
    ```

## 获取账户特定交易对余额

* Request

        POST /api/swap/v1/private/balance

* Parameters

    | Name | Type | Mandatory | Description |
    | --- | :---: | :---: | :--- |
    | symbol | string | YES | Symbol |

* Response

    ```json
    {
        "symbol": "BTC_USDT_P", 
        "currency": "USDT",                 // 币种
        "avail_margin": 11161897.76919204,  // 可用保证金
        "order_margin": 0.0,                // 订单保证金
        "frozen_margin": 1278.64010481,     // 已冻结保证金
        "balance": 11163176.40929685,       // 总余额
        "float": 8,                         // 显示精度
        "long_leverage": 10.0,              // 多仓杠杆倍数
        "short_leverage": 10.0,             // 空仓杠杆倍数
        "mode": "crossed",       // 仓位模式：crossed-全仓，fixed-逐仓
        "equal_btc": 1011.45962192          // Balance equal to btc
    }
    ```

## 获取账户所有交易对余额

* Request

        POST /api/swap/v1/private/balance/list

* Parameters

    Null

* Response

    ```json
    [
        {
            "symbol": "BTC_USDT_P", 
            "currency": "USDT",                 // Currency
            "avail_margin": 11161897.76919204,  // 可用保证金
            "order_margin": 0.0,                // 订单保证金
            "frozen_margin": 1278.64010481,     // 已冻结保证金
            "balance": 11163176.40929685,       // Total banlace
            "float": 8,                         // 显示精度
            "long_leverage": 10.0,              // 多仓杠杆倍数
            "short_leverage": 10.0,             // 空仓杠杆倍数
            "mode": 0,          // 仓位模式：crossed-全仓，fixed-逐仓
            "equal_btc": 1011.45962192          // Balance equal to btc
        }
    ]
    ```
