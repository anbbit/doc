# 私密接口

合约交易相关的信息，包括账户信息，订单操作，订单查询，账单明细查询。

## 字典

### 订单状态

	委托状态列表：
	pending_submit    // 下单已接收
	submitted         // 已提交, 待成交
	partial_filled    // 部分成交
	partial_canceled  // 部分成交已撤销
	filled            // 完全成交
	canceled          // 已撤销
	pending_cancel    // 撤销已提交
	sys_canceled      // canceled by system

	止盈止损状态列表：
	trigger_pending   // 订单正在等待触发
	trigger_failed    // 订单被触发，但执行失败（例如：冻结失败）
	trigger_canceled  // 订单未被触发而取消
	trigger_success   // 订单触发成功
	trigger_inactive  // 订单未激活，不会触发
	trigger_expired   // 订单已失效（没有持仓、等情况）

### 开仓方向 direction
	long  // 多
	short // 空

### 交易类型 trade_type
	openlong    // 买入开多
	openshort   // 卖出开空
	closeshort  // 买入平空
	closelong   // 卖出平多

### 订单类型（报价类型）
	limit   // 限价
	market  // 市价

### 仓位模式
	cross    // 全仓
	isolate  // 逐仓

## 获取当前账户信息

* 请求

        POST /api/swap/v1/private/account/list

* 参数

    无。

* 响应

    ```
    [
        {
            "symbol": "BTC_USDT_P", 
            "mode": "cross",                        // 仓位模式
            "account_value": 11163176.409296853,    // 账户权益
            "avail_margin": 11161897.76919204,      // 可用保证金
            "position_margin": 0,                   // 持仓保证金
            "order_margin": 0.0,                    // 挂单冻结保证金
            "unrealized_pnl": 0,                    // 未实现盈亏
            "realized_pnl": 0,                      // 已实现盈亏
            "transfer_balance": 11161897.76919204,  // 可划转余额
            "margin_rate": null,                    // 保证金率
            "maint_margin_rate": 0.3283,            // 维持保证金率
            "init_margin_rate": 0.3333,             // 初始保证金率
            "long_leverage": 10.0,                  // 多仓杠杠
            "short_leverage": 10.0,                 // 空仓杠杠
            "positions": [],                        // 持仓列表
            "unfilled_open_qty": 0.14               // 未成交的开仓数量
        }
    ]
    ```

## 获取当前账户状态

* 请求

        POST /api/swap/v1/private/account/state

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是  | 交易对名称 |

* 响应

    ```
    {
        "symbol": "BTC_USDT_P", 
        "mode": "cross",                         // 仓位模式
        "account_value": 11163176.409296853,     // 账户权益
        "avail_margin": 11161897.76919204,       // 可用保证金
        "position_margin": 0,                    // 持仓保证金
        "order_margin": 1278.64010481344,        // 挂单保证金
        "unrealized_pnl": 0,                     // 未实现盈亏
        "realized_pnl": 0,                       // 实现盈亏
        "transfer_balance": 11161897.76919204,   // 可划转余额
        "margin_rate": null,                     // 保证金率
        "maint_margin_rate": 0.045,              // 维持保证金率
        "init_margin_rate": 0.05,                // 初始保证金率
        "long_leverage": 10.0,                   // 多仓杠杠
        "short_leverage": 10.0,                  // 空仓杠杠
        "positions": [],                         // 持仓列表
        "unfilled_open_qty": 0.1                 // 未成交的开仓数量
    }
    ```

## 杠杆倍率修改

* 请求

        POST /api/swap/v1/private/account/leverage/update

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是  | 交易对名称，如：BTC_USDT_P |
    | leverage | float | 是 | 杠杆倍率 |
    | direction | string| 是 | 开仓方向，long-做多，short-做空 |

* 响应

    ```
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // 订单id
        "state": "submitted"    // 订单状态
    }
    ```

## 所有合约持仓信息

* 请求

        POST /api/swap/v1/private/position

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |

* 响应

    ```
    [
        {
            "position_id": 243, 
            "mode": "cross",                     // 仓位模式
            "symbol": "BTC_USDT_P", 
            "position_margin": 4317.842646366,   // 持仓保证金
            "init_margin": 31721.40101578,       // 初始保证金
            "order_margin": 0.0,                 // 挂单冻结保证金
            "unrealized_pnl": -274035.58369414,  // 未实现盈亏
            "realized_pnl": 0.0,                 // 已实现盈亏
            "pnl_rate": -8.638823473081134,      // 盈亏率
            "leverage": 10.0,                    // 杠杆倍率
            "direction": "long",                 // 持仓方向
            "qty": 3.927579,                     // 持仓数量
            "avail_qty": 3.927579,               // 可用数量
            "mark_price": 10993.64938647,        // 标记价格
            "avg_price": 80765.78731014705,      // 持仓均价
            "liquid_price": 0.0,                 // 强平价格
            "bankrupt_price": 0.0,               // 破产价格
            "amount": 317214.0101578,            // 开仓金额
            "mark_value": 43178.42646366,        // 持仓价值
            "margin_rate": null                  // 保证金率
        }
    ]
    ```

## 下单

创建新的合约订单。

* 请求

        POST /api/swap/v1/private/order/create

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | qty | float | 是 | 下单的数量 |
    | price | float | 是 | 下单的委托价格 |
    | trade_type | string | 是 | 下单的交易类型 |

* 响应

    ```
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // 订单id
        "state": "submitted"  // 订单状态
    }
    ```

## 批量下单

批量创建合约订单。

* 请求

        POST /api/swap/v1/private/order/create/batch

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | account_type | string | 是 | 账户类型：<br>swap-合约账户，<br>trading-币币账户，<br>spot_margin-杠杆账户 |
    | orders | list | 是 | 订单列表 |
    | cid | string | 是 | 用户自定义订单ID |
    | qty | float | 是 | 下单数量 |
    | price | float | 是 | 下单时的委托价格 |
    | trade_type| string | 是 | 下单的订单类型 |
    | amount | float  | 否 | 订单金额 |
    | type | string | 是 | 订单类型：limit-限价，market-市价 |

* 请求示例

        POST /api/swap/v1/private/order/create/batch

    ```
    {
        "symbol": "BTC_USDT_P",
        "account_type": "swap",
        "orders": [
            {
                "cid": "cid_0f48dd430da",  // 用户自定义订单ID
                "qty": 0.1,                // 下单数量
                "price": 10000,            // 委托价格
                "trade_type": "openlong",  // 交易类型
                "amount": "long",          // 订单金额
                "type": "limit",           // 订单类型
            },
        ]
    }
    ```

* 响应

    ```
    {
        "result": [
            {
                "order_id": "3.BTC_USDT_P.0.3448f869a1877429", // 订单id
                "state": "submitted"  // 订单状态
            }
        ]
    }
    ```

## 查询订单状态

* 请求

        POST /api/swap/v1/private/order/state

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | order_id | string | 是 | 订单id |

* 响应

    ```
    {
        "user_id": 1,                     // 用户id
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // 订单id
        "cid": "cid_2ccbdd58d5fa11ea943b00f48dd430da",  // 用户自定义订单ID
        "symbol": "BTC_USDT_P", 
        "side": "buy",             // 订单方向：buy-买单，sell-卖单
        "type": "limit",           // 订单类型
        "qty": 0.1,                // 订单数量
        "price": 10000.0,          // 委托价格
        "state": "submitted",      // 订单状态
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

* 请求

        POST /api/swap/v1/private/order/cancel

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | order_id | string | 是 | 订单id |

* 响应

    ```
    {
        "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17",  // 订单id
        "state": "pending_cancel"  // 订单状态
    }
    ```

## 撤掉所有订单

* 请求

        POST /api/swap/v1/private/order/cancel/all

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |

* 响应

    ```
    {
        "state": "pending_cancel"  // 订单状态
    }
    ```

## 批量撤单

* 请求

        POST /api/swap/v1/private/order/cancel/batch

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | order_ids | list | 是 | 订单id列表 |

* 响应

    ```
    {
        "result": [
            {
                "order_id": "3.BTC_USDT_P.0.f3c5c3ba08e40d17", // 订单id
                "state": "pending_cancel"    // 订单状态
            }
        ]
    }
    ```

## 获取当前委托订单

* 请求

        POST /api/swap/v1/private/order/active/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | account_type | string |  否 | 账户类型，如：swap_p（交割合约）|
    | order_type | string | 否 | 订单类型：limit-限价单，market-市价单 |
    | order_state | string | 否 | 订单状态 |
    | trade_type | string | 否 | 交易类型：<br>openlong-买入开多，<br>openshort-卖出开空，<br>closeshort-买入平空，<br>closelong-卖出平多 |
    | page | int | 否 | 页数 |
    | size | int | 否  | 每页显示的条目 |
    | start_tm_ms | int | 否 | 查询起始时间 |
    | end_tm_ms | int | 否 | 查询结束时间 |

* 响应

    ```
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
            "is_plan": false,               // 是否为计划委托
            "trigger_price": 0.0,           // 触发价格
            "state": "submitted",           // 订单状态
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

* 请求

        POST /api/swap/v1/private/order/history/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | account_type | string | 否 | 账户类型，如：swap_p（交割合约）|
    | order_type | string | 否 | 订单类型，如：limit（限价单）、market（市价单）|
    | order_state | string | 否 | 订单状态 |
    | trade_type | string | 否 | 交易类型：<br>openlong-买入开多、<br>openshort-卖出开空、<br>closeshort-买入平空、<br>closelong-卖出平多 |
    | page | int | 否 | 页数 |
    | size | int | 否 | 每页显示的条目 |
    | start_tm_ms | int | 否 | 查询起始时间 |
    | end_tm_ms | int | 否| 查询结束时间 |

* 响应

    ```
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
            "is_plan": false,               // 是否为计划委托
            "trigger_price": 0.0,           // 触发价格
            "state": "submitted",           // 订单状态
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

* 请求

        POST /api/swap/v1/private/trade/list

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |
    | page | int | 否 | 页数 |
    | size| int | 否 | 每页显示的数量 |
    | start_tm_ms | int | 否 | 查询起始时间 |
    | end_tm_ms | int | 否| 查询结束时间 |

* 响应

    ```
    {
        "page": 1,        // 当前页
        "list": [
            {
                "symbol": "BTC_USDT_P", 
                "leverage": 10.0,           // 杠杆倍数
                "trade_type": "openshort",  // 交易类型
                "qty": 0.001,               // 划转数量
                "price": 10969.5,           // 委托价格
                "is_maker": true,           // 是否挂单
                "is_buy_maker": false,      // 是否买入挂单
                "amount_fee": -0.0021939,   // 划转金额手续费
                "qty_fee": 0.0,             // 划转数量手续费
                "realized_pnl": 0.0,        // 实现盈亏
                "create_tm_ms": 1596110439268
            }
        ]
    }
    ```

## 获取账户特定交易对余额

* 请求

        POST /api/swap/v1/private/balance

* 参数

    | 名称 | 类型 | 必填 | 说明 |
    | --- | :---: | :---: | :--- |
    | symbol | string | 是 | 交易对名称 |

* 响应

    ```
    {
        "symbol": "BTC_USDT_P", 
        "currency": "USDT",                 // 币种
        "avail_margin": 11161897.76919204,  // 可用保证金
        "order_margin": 0.0,                // 订单保证金
        "balance": 11163176.40929685,       // 总余额
        "float": 8,                         // 显示精度
        "long_leverage": 10.0,              // 多仓杠杆倍数
        "short_leverage": 10.0,             // 空仓杠杆倍数
        "mode": "cross",                    // 仓位模式
        "equal_btc": 1011.45962192          // 余额转btc
    }
    ```

## 获取账户所有交易对余额

* 请求

        POST /api/swap/v1/private/balance/list

* 参数

    无

* 响应

    ```
    [
        {
            "symbol": "BTC_USDT_P", 
            "currency": "USDT",                 // 币种
            "avail_margin": 11161897.76919204,  // 可用保证金
            "order_margin": 0.0,                // 订单保证金
            "balance": 11163176.40929685,       // 总余额
            "float": 8,                         // 显示精度
            "long_leverage": 10.0,              // 多仓杠杆倍数
            "short_leverage": 10.0,             // 空仓杠杆倍数
            "mode": "cross",                    // 仓位模式
            "equal_btc": 1011.45962192          // 余额转btc
        }
    ]
    ```
