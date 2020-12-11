# 交易接口

## API说明

### 服务地址

    https://api.anbbit.com

### 签名认证

* 如果您没有API_KEY_ID和API_KEY_SECRET，请前往个人中心申请
* 使用密钥API_KEY_SECRET和算法hmac对消息体进行SHA256签名，获取HEX格式字符串作为签名结果

#### 密钥

* API_KEY_SECRET转为字节

#### 被签名的数据

* 被签名的数据主要是放在消息体中的json参数
* 必须包含时间参数nonce，值为毫秒时间戳
* 将消息体转为字节

```
// 签名数据json示例
{
  "param_1_key": "param_1_value",
  "nonce": 1519808456000
}
```

#### 签名示例python伪代码

```python
import json
import hmac
import requests

from hashlib import sha256

API_KEY_ID = ''
API_KEY_SECRET = ''

# 必需时间参数nonce
params = {
    "nonce": 1519808456000
}

params_str = json.dumps(params, separators=(',', ':'))

signature = hmac.new(API_KEY_SECRET.encode('utf8'), params_str.encode('utf8'), sha256).hexdigest()

headers = {
    'api_key': API_KEY_ID,
    'signature': signature
}
url = ''
ret = requests.post(url, json=None, data=params_str, headers=headers, cookies=None)

```

#### 请求Header

* api_key: API_KEY_ID
* signature: 签名结果

### 请求频率限制

默认10秒内最大请求量100

### 接口响应

* 所有接口返回数据均为application/json
* status=0表示成功, 其他状态参考具体的接口
* data存放需要返回具体的数据，如果无需返回数据，data可能不存在

```
{
    "status": 0,
    "msg": "ok",
    "data": {}
  }
```

## 公开接口

参考 [Public-Interface.md](./Public-Interface.md)

## 私密接口

参考 [Private-Interface.md](./Private-Interface.md)
