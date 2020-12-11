# Exchange API

## API Description

### Host

    https://api.anbbit.com

### Signature

* If you don't have the API_KEY_ID and API_KEY_SECRET, apply for them in personal center.
* The message body is signed with SHA256 using the key API_KEY_SECRET and the algorithm HMAC, and the HEX format string is obtained as the result of the signature

#### Secret Key

* API_KEY_SECRET convert to byte.

#### Signed Data

* The signed data is primarily a JSON parameter placed in the body of the message.
* Must contain the time parameter nonce, with a value of milliseconds timestamp.
* Converts the message body to bytes.

```
// Json example of signed data
{
  "param_1_key": "param_1_value",
  "nonce": 1519808456000
}
```

#### Signing sample Python pseudocode

```python
import json
import hmac
import requests

from hashlib import sha256

API_KEY_ID = ''
API_KEY_SECRET = ''

# Mandatory parameter nonce
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

#### Request Header

* api_key: API_KEY_ID
* signature: signed result

### Request frequency limit

The default maximum number of requests is 100 in 10 seconds.

### Response

* All interface return data is Application/JSON.
* Status =0 indicates success, other states refer to the specific interface.
* Data storage needs to return specific data, if there is no need to return data, data may not exist.

```
{
    "status": 0,
    "msg": "ok",
    "data": {}
  }
```

## Public API

Reference [Public-Interface.md](./Public-Interface.md)

## Private API

Reference [Private-Interface.md](./Private-Interface.md)
