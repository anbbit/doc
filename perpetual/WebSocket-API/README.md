# Websocket 合约行情推送

## 订阅地址

    wss://ws.anbbit.com/swap/v2/sub

### 实时订阅/取消

* 	订阅请求基本格式

    `{"type": "sub", "chan": "{chan_id}"}`

* 取消订阅基本格式

    `{"type":"unsub", "chan":"{chan_id}"}`
	
* 订阅分为4个大类:

    | 类别 | chan_id格式 | 订阅请求示例 |  功能描述 |
    | --- | :--: | :--: | :-- |
    | trade | trade.{symbol} | {"type":"sub","chan":"trade.BTC_USDT_P"} | 成交记录 |
    | orderbook | orderbook.{symbol}.{level} | {"type":"sub","chan":"orderbook.BTC_USDT_P.L20"}  | 活跃订单簿 |
    | kline | kline.{symbol}.{kline_interval} | {"type":"sub","chan":"kline.BTC_USDT_P.1m"}  | K线 |
    | ticker | ticker.{symbol} | {"type":"sub","chan":"ticker.BTC_USDT_P"} |  盘口 |

* symbol可以通过**交易对列表接口**获取支持的交易对
* kline_interval支持列表:
    `1m、3m、5m、15m、30m、1h、2h、4h、6h、8h、12h、1d、3d、1w、1M`
* level 支持的列表:
    `L20、L50`
	
### 服务端返回数据的解压缩

* 服务端返回数据是gzip格式，需要解压，解压后为json数据，Python示例如下：
    `gzip.decompress(data).decode('utf-8')`

### 活跃订单簿

* ordbk_type: d-增量，f-全量
* 应答示例

    ```
    {
        "asks":[
            19436.89,   // 卖一价
            1.19,       // 卖一量
            19437.18,   // 卖二价
            0.809       // 卖二量
        ],
        "bids":[
            19434.2,    // 买一价
            0.054115,   // 买一量
            19433.66,   // 买二价
            0.395       // 买二量
        ],
        "seq":1606431031357,
        "type":"orderbook.BTC_USDT_P.L50",
        "symbol":"BTC_USDT_P",
        "ordbk_type":"d"
    }
    ```

### 成交记录

* 应答示例

    ```
    {
        "trade_id":"d647021833ca11eba55c06258d30ec96",
        "side":"buy",
        "price":19436.89,
        "qty":0.001,
        "ts":1606823183631,
        "type":"trade.BTC_USDT_P"
    }
    ```

### 盘口

* 应答示例

    ```
    {
        "type":"ticker.BTC_USDT_P",
        "seq":6907,
        "ticker":[
            14000.0,   // 最新一笔成交的成交价
            0.04,      // 最近一笔成交的成交量
            16000.0,   // 最小卖一价
            1.0,       // 最小卖一量
            15000.0,   // 最大买一价
            0.510964,  // 最大买一量
            11.0,      // 24小时前成交价
            14000.0,   // 24小时内最高价
            11.0,      // 24小时内最低价
            1.233,     // 24小时内基准货币成交量
            2845.06118 // 24小时内计价货币成交量
        ]
    }
    ```

### k线

* 应答示例

    ```
    {
        "data":[
            {
                "id": 1572498120,      // k线时间段
                "seq": 10,             // 序号
                "high": 14000.0,       // 最高价
                "low": 10060.5,        // 最低价
                "open": 10060.5,       // 起始价
                "close": 14000.0,      // 结束价
                "count": 7,            // 成交笔数
                "base_vol": 0.161,     //基准货币成交量
                "quote_vol": 2111.2795 // 计价货币成交量
            }
        ],
        "type":"kline.BTC_USDT_P.1m"
    }
    ```
