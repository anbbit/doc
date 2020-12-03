# Spot Market API

## API Description

### API Host

    https://market.anbbit.com

## Aggregate Trades Record

    GET /bitan/api/v3/aggTrades

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |
| limit | INT | NO | Default 500; max 1000. |

### Sample

    GET https://market.anbbit.com/bitan/api/v3/aggTrades?symbol=BTCUSDT

```json
[
    {
        "a": 26129,         // Aggregate tradeId
        "p": "0.01633102",  // Price
        "q": "4.70443515",  // Quantity
        "f": 27781,         // First tradeId
        "l": 27781,         // Last tradeId
        "T": 1498793709153, // Timestamp
        "m": true,          // Was the buyer the maker?
        "M": true           // Was the trade the best price match?
    }
]
```

## Active Order Book

    GET /bitan/api/v3/depth

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |
| limit | INT | NO | Default 500; max 1000. |

### Sample

    GET https://market.anbbit.com/bitan/api/v3/depth?symbol=BTCUSDT

```json
{
  "lastUpdateId": 1027024,
  "bids": [
    [
      "4.00000000",     // PRICE
      "431.00000000"    // QTY
    ]
  ],
  "asks": [
    [
      "4.00000200",     // PRICE
      "12.00000000"     // QTY
    ]
  ]
}
```

## 24hr Ticker Price Change Statistics

    GET /bitan/api/v3/ticker/24hr

### Parameters

Null.

### Sample

    GET https://market.anbbit.com/bitan/api/v3/ticker/24hr

```json
[
  {
    "symbol": "BNBBTC",
    "priceChange": "-94.99999800",
    "priceChangePercent": "-95.960",
    "weightedAvgPrice": "0.29628482",
    "prevClosePrice": "0.10002000",
    "lastPrice": "4.00000200",
    "lastQty": "200.00000000",
    "bidPrice": "4.00000000",
    "askPrice": "4.00000200",
    "openPrice": "99.00000000",
    "highPrice": "100.00000000",
    "lowPrice": "0.10000000",
    "volume": "8913.30000000",
    "quoteVolume": "15.30000000",
    "openTime": 1499783499040,
    "closeTime": 1499869899040,
    "firstId": 28385,     // First tradeId
    "lastId": 28460,      // Last tradeId
    "count": 76           // Trade count
  }
]
```

## K-line

    GET /bitan/api/v3/klines

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |
| interval | STRING | YES | Valid list: 1m,3m,5m,15m,30m,1h,2h,4h,6h,8h,12h,1d,3d,1w,1M |
| limit | INT | NO | Default 500; max 1000. |
| startTime | INT | NO | Timestamp in ms |
| endTime | INT | NO | Timestamp in ms |

### Sample

    GET https://market.anbbit.com/bitan/api/v3/klines?symbol=BTCUSDT&interval=1m&limit=10

```json
[
    [
        1499040000000,      // Open time
        "0.01634790",       // Open
        "0.80000000",       // High
        "0.01575800",       // Low
        "0.01577100",       // Close
        "148976.11427815",  // Volume
        1499644799999,      // Close time
        "2434.19055334",    // Quote asset volume
        308,                // Number of trades
        "1756.87402397",    // Taker buy base asset volume
        "28.46694368",      // Taker buy quote asset volume
        "17928899.62484339" // Ignore.
    ]
]
```
