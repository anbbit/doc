# 合约行情API

## API说明

### 服务地址

    https://market.anbbit.com

## 活跃订单簿

    GET /api/swap/v1/public/market/orderbook/{symbol}/{level}

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | :---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |
| level | STRING | 是 | 交易级别，支持的级别：L20、L50 |

### 示例

    GET https://market.anbbit.com/api/swap/v1/public/market/orderbook/BTC_USDT_P/ALL

```
{
    "status": 0,
    "msg": "ok",
    "data": {
        "type": "depth.L20.BTC_USDT_P",
        "symbol": "BTC_USDT_P",
        "ts": 1572490711353,  // 时间戳
        "seq": 1572448705737, // 序号
        "asks": [
            10060.5,          // 卖一价
            0.001,            // 卖一量
            10060.9,          // 卖二价
            0.01              // 卖二量
        ],
        "bids": [
            10060.4,          // 买一价
            0.009,            // 买一量
            10060.3,          // 买二价
            0.01              // 买二量
        ]
    }
}
```

## 成交记录

    GET /api/swap/v1/public/market/trade/{symbol}

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | :---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |

### 示例

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

## 盘口

    GET /api/swap/v1/public/market/ticker/{symbol}

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | :---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |

### 示例

    GET https://market.anbbit.com/api/swap/v1/public/market/ticker/BTC_USDT_P

```
{
    "status": 0,
    "msg": "ok",
    "data": {
        "symbol": "BTC_USDT_P",
        "seq": 25,
        "ticker": [
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
}
```

## k线

    GET /api/swap/v1/public/market/kline/{symbol}/{interval}

### 参数说明

| 名称 | 类型 | 必填 | 说明 |
| --- | :---: | :---: | :--- |
| symbol | STRING | 是 | 交易对名称，通过交易对列表获取支持的交易对 |
| interval | STRING | 是 | 间隔，当前支持1m、3m、5m、15m、30m、<br>1h、2h、4h、6h、8h、12h、1d、3d、1w、1M |
| begin | INT | 是 | 起始时间戳，单位：秒 |
| end | INT | 是 | 终止时间戳，单位：秒 |

### 示例

    GET https://market.anbbit.com/api/swap/v1/public/market/kline/BTC_USDT_P/1m?begin=1572492685&end=1572499129

```
{
    "status": 0,
    "msg": "ok",
    "data": [     // 按时间升序排序
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
    ]
}
```
