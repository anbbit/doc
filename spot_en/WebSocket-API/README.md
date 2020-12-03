# Spot Market Websocket

## Websocket Host

    wss://ws.anbbit.com/bitan/stream

### Subscribe/Unsubscribe

* The following data can be sent through the websocket instance in order to subscribe/unsubscribe from streams. Examples can be seen below.
* The id used in the JSON payloads is an unsigned INT used as an identifier to uniquely identify the messages going back and forth.
* In the response, if the result received is null this means the request sent was a success.

#### Subscribe

* Request

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

* Response

    ```
    {
        "result": null,
        "id": 1
    }
    ```

#### Unsubscribe

* Request

    ```
    {
        "method": "UNSUBSCRIBE",
        "params": [
            "btcusdt@depth"
        ],
        "id": 312
    }
    ```

* Response

    ```
    {
        "result": null,
        "id": 312
    }
    ```

#### Aggregate Trade

* Stream Name: \<symbol\>@aggTrade
* Update Speed: Real-time
* Payload: 

    ```
    {
        "e": "aggTrade",  // Event type
        "E": 123456789,   // Event time
        "s": "BNBBTC",    // Symbol
        "a": 12345,       // Aggregate trade ID
        "p": "0.001",     // Price
        "q": "100",       // Quantity
        "f": 100,         // First trade ID
        "l": 105,         // Last trade ID
        "T": 123456785,   // Trade time
        "m": true,        // Is the buyer the market maker?
        "M": true         // Ignore
    }
    ```

#### K-line

* The Kline Stream push updates to the current klines every second.
* Stream Name: \<symbol\>@kline_\<interval\>
* Update Speed: 2000ms
* Kline intervals:
    m -> minutes; h -> hours; d -> days; w -> weeks; M -> months
* Kline interval valid list:
    1m、3m、5m、15m、30m、1h、2h、4h、6h、8h、12h、1d、3d、1w、1M
* Payload: 
    
    ```
    {
        "e": "kline",     // Event type
        "E": 123456789,   // Event time
        "s": "BNBBTC",    // Symbol
        "k": {
            "t": 123400000, // Kline start time
            "T": 123460000, // Kline close time
            "s": "BNBBTC",  // Symbol
            "i": "1m",      // Interval
            "f": 100,       // First trade ID
            "L": 200,       // Last trade ID
            "o": "0.0010",  // Open price
            "c": "0.0020",  // Close price
            "h": "0.0025",  // High price
            "l": "0.0015",  // Low price
            "v": "1000",    // Base asset volume
            "n": 100,       // Number of trades
            "x": false,     // Is this kline closed?
            "q": "1.0000",  // Quote asset volume
            "V": "500",     // Taker buy base asset volume
            "Q": "0.500",   // Taker buy quote asset volume
            "B": "123456"   // Ignore
        }
    }
    ```

#### All Market Mini Tickers

* 24hr rolling window mini-ticker statistics for all symbols that changed in an array. These are NOT the statistics of the UTC day, but a 24hr rolling window for the previous 24hrs. Note that only tickers that have changed will be present in the array.
* Stream Name: !miniTicker@arr
* Update Speed: 1000ms
* Payload: 
    
    ```
    [
        {
            "e": "24hrMiniTicker",  // Event type
            "E": 123456789,         // Event time
            "s": "BNBBTC",          // Symbol
            "c": "0.0025",          // Close price
            "o": "0.0010",          // Open price
            "h": "0.0025",          // High price
            "l": "0.0010",          // Low price
            "v": "10000",           // Total traded base asset volume
            "q": "18"               // Total traded quote asset volume
        }
    ]
    ```

#### Partial Book Depth

* Order book price and quantity depth updates used to locally manage an order book.
* Stream Name: \<symbol\>@depth OR \<symbol\>@depth@100ms
* Update Speed: 1000ms OR 100ms
* Payload: 
    
    ```
    {
        "e": "depthUpdate", // Event type
        "E": 123456789,     // Event time
        "s": "BNBBTC",      // Symbol
        "U": 157,           // First update ID in event
        "u": 160,           // Final update ID in event
        "b": [              // Bids to be updated
            [
                "0.0024",       // Price level to be updated
                "10"            // Quantity
            ]
        ],
        "a": [              // Asks to be updated
            [
                "0.0026",       // Price level to be updated
                "100"           // Quantity
            ]
        ]
    }
    ```

#### Error Messages

| Error Message | Description |
| --- | --- |
| {"code": 0, "msg": "Unknown property"} | Parameter used in the SET_PROPERTY or GET_PROPERTY was invalid |
| {"code": 1, "msg": "Invalid value type: expected Boolean"} | Value should only be true or false |
| {"code": 2, "msg": "Invalid request: property name must be a string"} | 	Property name provided was invalid |
| {"code": 2, "msg": "Invalid request: request ID must be an unsigned integer"} | Parameter id had to be provided or the value provided in the id parameter is an unsupported type |
| {"code": 2, "msg": "Invalid request: unknown variant %s, expected one of SUBSCRIBE, UNSUBSCRIBE, LIST_SUBSCRIPTIONS, SET_PROPERTY, GET_PROPERTY at line 1 column 28"} | Possible typo in the provided method or provided method was neither of the expected values |
| {"code": 2, "msg": "Invalid request: too many parameters"} | Unnecessary parameters provided in the data |
| {"code": 2, "msg": "Invalid request: property name must be a string"} | Property name was not provided |
| {"code": 2, "msg": "Invalid request: missing field method at line 1 column 73"}	 | method was not provided in the data |
| {"code":3,"msg":"Invalid JSON: expected value at line %s column %s"} | JSON data sent has incorrect syntax. |
