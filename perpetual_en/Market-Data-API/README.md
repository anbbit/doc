# Perpetual Market API

## API Description

### Host

    https://market.anbbit.com

## Active Order Book

    GET /api/swap/v1/public/market/orderbook/{symbol}/{level}

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |
| level | STRING | YES | Order book depth, valid list: L20, L50 |

### Sample

    GET https://market.anbbit.com/api/swap/v1/public/market/orderbook/BTC_USDT/L20

```
{
    "status": 0,
    "msg": "ok",
    "data": {
        "type": "depth.L20.BTC_USDT_P",
        "symbol": "BTC_USDT_P",
        "ts": 1572490711353,  // Timestamp in ms
        "seq": 1572448705737,
        "asks": [
            19436.89,   // Price level to be updated
            1.19,       // Quantity
            19437.18,   // Price level to be updated
            0.809       // Quantity
        ],
        "bids": [
            19434.2,    // Price level to be updated
            0.054115,   // Quantity
            19433.66,   // Price level to be updated
            0.395       // Quantity
        ]
    }
}
```

## Trade Record

    GET /api/swap/v1/public/market/trade/{symbol}

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |

### Sample

    GET https://market.anbbit.com/api/swap/v1/public/market/trade/BTC_USDT_P

```
{
    "status": 0,
    "msg": "ok",
    "data": [
        {
            "trade_id": "52220f42fb8a11e9b84c02a7107ba21a",
            "side": "buy",
            "price": 10060.4,
            "qty": 0.001,
            "ts": 1572490711353
        }
    ]
}
```

## Ticker

    GET /api/swap/v1/public/market/ticker/{symbol}

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |

### Sample

    GET https://market.anbbit.com/api/swap/v1/public/market/ticker/BTC_USDT_P

```
{
    "status": 0,
    "msg": "ok",
    "data": {
        "symbol": "BTC_USDT_P",
        "seq": 25,
        "ticker": [
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
}
```

## K-line

    GET /api/swap/v1/public/market/kline/{symbol}/{interval}

### Parameters

| Name | Type | Mandatory | Description |
| --- | :---: | :---: | :--- |
| symbol | STRING | YES | Get the valid symbol list from **Symbol list endpoint** |
| interval | STRING | YES | Valid list: 1m、3m、5m、15m、30m、<br>1h、2h、4h、6h、8h、12h、1d、3d、1w、1M |
| begin | INT | YES | Timestamp in second to get list from INCLUSIVE. |
| end | INT | YES | Timestamp in second to get list until INCLUSIVE. |

### Sample

    GET https://market.anbbit.com/api/swap/v1/public/market/kline/BTC_USDT_P/1m?begin=1572492685&end=1572499129

```
{
    "status": 0,
    "msg": "ok",
    "data": [
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
    ]
}
```
