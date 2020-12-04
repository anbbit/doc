# 现货行情API

## API说明

### 服务地址

    https://market.anbbit.com

## 近期成交(归集)

    GET /bitan/api/v3/aggTrades

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | :---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |
| limit | INT | 否 | 查询数量限制，默认 500；最大 1000 |

### 示例

    GET https://market.anbbit.com/bitan/api/v3/aggTrades?symbol=BTCUSDT

```
[
    {
        "a": 26129,         // 归集成交ID
        "p": "0.01633102",  // 成交价
        "q": "4.70443515",  // 成交量
        "f": 27781,         // 被归集的首个成交ID
        "l": 27781,         // 被归集的末个成交ID
        "T": 1498793709153, // 成交时间
        "m": true,          // 是否为主动卖出单
        "M": true           // 是否为最优撮合单(可忽略，目前总为最优撮合)
    }
]
```

## 深度信息

    GET /bitan/api/v3/depth

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | :---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |
| limit | INT | 否 | 查询数量限制，默认 500；最大 1000 |

### 示例

    GET https://market.anbbit.com/bitan/api/v3/depth?symbol=BTCUSDT

```
{
  "lastUpdateId": 1027024,
  "bids": [
    [
      "4.00000000",     // 价位
      "431.00000000"    // 挂单量
    ]
  ],
  "asks": [
    [
      "4.00000200",     // 价位
      "12.00000000"     // 挂单量
    ]
  ]
}
```

## 24hr 价格变动情况

    GET /bitan/api/v3/ticker/24hr

### 参数说明

无。

### 示例

    GET https://market.anbbit.com/bitan/api/v3/ticker/24hr

```
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
    "firstId": 28385,     // 首笔成交id
    "lastId": 28460,      // 末笔成交id
    "count": 76           // 成交笔数
  }
]
```

## k线

    GET /bitan/api/v3/klines

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | ---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |
| interval | STRING | 是 | 间隔，当前支持1m、3m、5m、15m、30m、<br>1h、2h、4h、6h、8h、12h、1d、3d、1w、1M |
| limit | INT | 否 | 查询数量限制，默认 500；最大 1000 |
| startTime | INT | 否 | 起始时间戳，单位：毫秒 |
| endTime | INT | 否 | 终止时间戳，单位：毫秒 |

### 示例

    GET https://market.anbbit.com/bitan/api/v3/klines?symbol=BTCUSDT&interval=1m&limit=10

```
[
    [
        1499040000000,      // 开盘时间
        "0.01634790",       // 开盘价
        "0.80000000",       // 最高价
        "0.01575800",       // 最低价
        "0.01577100",       // 收盘价(当前K线未结束的即为最新价)
        "148976.11427815",  // 成交量
        1499644799999,      // 收盘时间
        "2434.19055334",    // 成交额
        308,                // 成交笔数
        "1756.87402397",    // 主动买入成交量
        "28.46694368",      // 主动买入成交额
        "17928899.62484339" // 请忽略该参数
    ]
]
```
