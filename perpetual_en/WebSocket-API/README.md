# Perpetual Market Websocket

## Websocket Host

    wss://ws.anbbit.com/swap/v2/sub

### Subscribe/Unsubscribe

* 	Subscribe base format

    `{"type": "sub", "chan": "{chan_id}"}`

* Unsubscribe base format

    `{"type":"unsub", "chan":"{chan_id}"}`
	
* Four category of subscription:

    | Category | chan_id format | Sample |  Description |
    | --- | :--: | :--: | :-- |
    | trade | trade.{symbol} | {"type":"sub","chan":"trade.BTC_USDT_P"} | Trade record |
    | orderbook | orderbook.{symbol}.{level} | {"type":"sub","chan":"orderbook.BTC_USDT_P.L20"}  | Active order book |
    | kline | kline.{symbol}.{kline_interval} | {"type":"sub","chan":"kline.BTC_USDT_P.1m"}  | K-line |
    | ticker | ticker.{symbol} | {"type":"sub","chan":"ticker.BTC_USDT_P"} | Ticker |

* You can get the valid symbol list from **Symbol list endpoint**
* kline_interval valid list:
    `1m、3m、5m、15m、30m、1h、2h、4h、6h、8h、12h、1d、3d、1w、1M`
* level valid list:
    `L20、L50`
	
### Decompress Data

* Published data is gzip format，get json format after decompress，Python sample code：
    `gzip.decompress(data).decode('utf-8')`

### Active Order Book

* ordbk_type: d-diff, f-full
* Response

    ```
    {
        "asks":[
            19436.89,   // Price level to be updated
            1.19,       // Quantity
            19437.18,   // Price level to be updated
            0.809       // Quantity
        ],
        "bids":[
            19434.2,    // Price level to be updated
            0.054115,   // Quantity
            19433.66,   // Price level to be updated
            0.395       // Quantity
        ],
        "seq":1606431031357,
        "type":"orderbook.BTC_USDT_P.L50",
        "symbol":"BTC_USDT_P",
        "ordbk_type":"d"
    }
    ```

### Trade Record

* Response

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

### Ticker

* Response

    ```
    {
        "type":"ticker.BTC_USDT_P",
        "seq":6907,
        "ticker":[
            14000.0,   // Last price
            0.04,      // Last quantity
            16000.0,   // Best bid price
            1.0,       // Best bid quantity
            15000.0,   // Best ask price
            0.510964,  // Best ask quantity
            11.0,      // Open price
            14000.0,   // High price
            11.0,      // Low price
            1.233,     // Total traded base asset volume
            2845.06118 // Total traded quote asset volume
        ]
    }
    ```

### k-line

* Response

    ```
    {
        "data":[
            {
                "id": 1572498120,      // Kline start time
                "seq": 10,             // Number of trades
                "high": 14000.0,       // High price
                "low": 10060.5,        // Low price
                "open": 10060.5,       // Open price
                "close": 14000.0,      // Close price
                "count": 7,            // Trade count
                "base_vol": 0.161,     // Base asset volume
                "quote_vol": 2111.2795 // Quote asset volume
            }
        ],
        "type":"kline.BTC_USDT_P.1m"
    }
    ```
