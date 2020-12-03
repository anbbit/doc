# Websocket 现货行情推送

## 订阅地址

    wss://ws.anbbit.com/bitan/stream

### 实时订阅/取消数据流

* 以下数据可以通过websocket发送以实现订阅或取消订阅数据流。示例如下。
* 响应内容中的id是无符号整数，作为往来信息的唯一标识。
* 如果相应内容中的 result 为 null，表示请求发送成功。

#### 订阅一个信息流

* 请求

    ```
    {
        "method": "SUBSCRIBE",
        "params": [
            "btcusdt@aggTrade",
            "btcusdt@depth"
        ],
        "id": 1
    }
    ```

* 响应

    ```
    {
        "result": null,
        "id": 1
    }
    ```

#### 取消订阅一个信息流

* 请求

    ```
    {
        "method": "UNSUBSCRIBE",
        "params": [
            "btcusdt@depth"
        ],
        "id": 312
    }
    ```

* 响应

    ```
    {
        "result": null,
        "id": 312
    }
    ```

#### 归集交易流

* 信息流: \<symbol\>@aggTrade
* 更新速度: 实时
* 信息内容: 

    ```
    {
        "e": "aggTrade",  // 事件类型
        "E": 123456789,   // 事件时间
        "s": "BNBBTC",    // 交易对
        "a": 12345,       // 归集交易ID
        "p": "0.001",     // 成交价格
        "q": "100",       // 成交笔数
        "f": 100,         // 被归集的首个交易ID
        "l": 105,         // 被归集的末次交易ID
        "T": 123456785,   // 成交时间
        "m": true,        // 买方是否是做市方。如true，则此次成交是一个主动卖出单，否则是一个主动买入单。
        "M": true         // 请忽略该字段
    }
    ```

#### K线

* K线信息流逐秒推送所请求的K线种类(最新一根K线)的更新。
* 信息流: \<symbol\>@kline_\<interval\>
* 更新速度: 2000ms
* K线图间隔参数:
    m -> 分钟; h -> 小时; d -> 天; w -> 周; M -> 月
* K线图间隔支持列表:
    1m、3m、5m、15m、30m、1h、2h、4h、6h、8h、12h、1d、3d、1w、1M
* 信息内容: 
    
    ```
    {
        "e": "kline",     // 事件类型
        "E": 123456789,   // 事件时间
        "s": "BNBBTC",    // 交易对
        "k": {
            "t": 123400000, // 这根K线的起始时间
            "T": 123460000, // 这根K线的结束时间
            "s": "BNBBTC",  // 交易对
            "i": "1m",      // K线间隔
            "f": 100,       // 这根K线期间第一笔成交ID
            "L": 200,       // 这根K线期间末一笔成交ID
            "o": "0.0010",  // 这根K线期间第一笔成交价
            "c": "0.0020",  // 这根K线期间末一笔成交价
            "h": "0.0025",  // 这根K线期间最高成交价
            "l": "0.0015",  // 这根K线期间最低成交价
            "v": "1000",    // 这根K线期间成交量
            "n": 100,       // 这根K线期间成交笔数
            "x": false,     // 这根K线是否完结(是否已经开始下一根K线)
            "q": "1.0000",  // 这根K线期间成交额
            "V": "500",     // 主动买入的成交量
            "Q": "0.500",   // 主动买入的成交额
            "B": "123456"   // 忽略此参数
        }
    }
    ```

#### 全市场所有Symbol的精简Ticker

* 最近24小时精简ticker信息
* 信息流: !miniTicker@arr
* 更新速度: 1000ms
* 信息内容: 
    
    ```
    [
        {
            "e": "24hrMiniTicker",  // 事件类型
            "E": 123456789,         // 事件时间
            "s": "BNBBTC",          // 交易对
            "c": "0.0025",          // 最新成交价格
            "o": "0.0010",          // 24小时前开始第一笔成交价格
            "h": "0.0025",          // 24小时内最高成交价
            "l": "0.0010",          // 24小时内最低成交加
            "v": "10000",           // 成交量
            "q": "18"               // 成交额
        }
    ]
    ```

#### 增量深度信息

* 每秒或每100毫秒推送orderbook的变化部分(如果有)
* 信息流: \<symbol\>@depth 或 \<symbol\>@depth@100ms
* 更新速度: 1000ms 或 100ms
* 信息内容: 
    
    ```
    {
        "e": "depthUpdate", // 事件类型
        "E": 123456789,     // 事件时间
        "s": "BNBBTC",      // 交易对
        "U": 157,           // 从上次推送至今新增的第一个 update Id
        "u": 160,           // 从上次推送至今新增的最后一个 update Id
        "b": [              // 变动的买单深度
            [
                "0.0024",       // 变动的价格档位
                "10"            // 数量
            ]
        ],
        "a": [              // 变动的卖单深度
            [
                "0.0026",       // 变动的价格档位
                "100"           // 数量
            ]
        ]
    }
    ```

#### 错误信息

| 错误信息 | 描述 
| --- | --- |
| {"code": 0, "msg": "Unknown property"} | SET_PROPERTY 或 GET_PROPERTY中应用的参数无效 |
| {"code": 1, "msg": "Invalid value type: expected Boolean"} | 仅接受true或false |
| {"code": 2, "msg": "Invalid request: property name must be a string"} | 	提供的属性名无效 |
| {"code": 2, "msg": "Invalid request: request ID must be an unsigned integer"} | 参数id未提供或id值是无效类型 |
| {"code": 2, "msg": "Invalid request: unknown variant %s, expected one of SUBSCRIBE, UNSUBSCRIBE, LIST_SUBSCRIPTIONS, SET_PROPERTY, GET_PROPERTY at line 1 column 28"} | 错字提醒，或提供的值不是预期类型 |
| {"code": 2, "msg": "Invalid request: too many parameters"} | 数据中提供了不必要参数 |
| {"code": 2, "msg": "Invalid request: property name must be a string"} | 未提供属性名 |
| {"code": 2, "msg": "Invalid request: missing field method at line 1 column 73"}	 | 数据未提供method |
| {"code":3,"msg":"Invalid JSON: expected value at line %s column %s"} | JSON 语法有误. |
