## 请求限速插件

该插件请求 `Rate Limiting`。

### 开启插件

```shell
curl -XPOST http://127.0.0.1:7777/rate_limiting/enable -d "enable=1"
```

### 为插件添加 Selectors

```shell
curl http://127.0.0.1:7777/rate_limiting/selectors -X POST -d '
{
    "name": "signature_auth-selectors",
    "type": 0,
    "judge": {},
    "handle": {
        "continue": true,
        "log": false
    },
    "enable": true
}'
```

| 参数名称        | 参数描述   |
|----------------|-----------|
|name            | 选择器名称。|
|type            | 选择器类型, 值为 `0` 时表示 `全部流量` ，为 `1` 时表示 `自定义流量` 。 |
|handle.continue | 选择器动作, 值为 `true` 时表示 `继续后续选择器`，为 `false` 时表示 `略过后续选择器`。 |
|handle.log      | 选择器日志, 值为 `true` 时表示 `记录日志`，为 `false` 时表示 `不记录日志`。 |

### 为插件添加 URI

```shell
curl http://127.0.0.1:7777/signature_auth/selectors/{selector_id}/rules -X POST -d
{
    "name": "signature_auth-plugin",
    "judge": {
        "type": 0,
        "conditions": [
            {
                "type": "URI",
                "operator": "=",
                "value": "/redirect_signature_auth"
            }
        ]
    },
    "extractor": {
    },
    "handle": {
        "period": 60,
        "count": 2,
        "log": true
    },
    "enable": true
}
```

| 参数名称        | 参数描述       |
|----------------|---------------|
|handle.period   | 时间周期。|
|handle.count    | 时间周期内最大访问次数。|
|handle.log      | 是否记录日志，值为 `true` 表示 `记录日志`，为 `false` 表示 `不记录日志`。 |

### 测试插件

```shell
curl -X GET http://127.0.0.1/plugin_rate_limiting
HTTP/1.1 200 OK

curl -X GET http://127.0.0.1/plugin_rate_limiting
HTTP/1.1 200 OK

...

curl -X GET http://127.0.0.1/plugin_rate_limiting
HTTP/1.1 429 Too Many Requests
```

### 关闭插件

```shell
curl -XPOST http://127.0.0.1:7777/signature_auth/enable -d "enable=0"
```
