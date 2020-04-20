## Key Auth Plugin

Used to `Key Auth` requests authentication.

### Enable Plugin

```shell
curl -XPOST http://127.0.0.1:7777/key_auth/enable -d "enable=1"
```

### Add Selectors to Plugin

```shell
curl http://127.0.0.1:7777/key_auth/selectors -X POST -d '
{
    "name": "key_auth-selectors",
    "type": 0,
    "judge": {},
    "handle": {
        "continue": true,
        "log": false
    },
    "enable": true
}'
```

| Params Name    | Params Description |
|----------------|--------------------|
|name            | selectors name. |
|type            | selectors type, value of `0` indicates `all request` and `1` indicates `custom request`. |
|handle.continue | selectors action, value of `true` indicates `continue selector` and `false` indicates  `skip selector`. |
|handle.log      | selectors log, value of `true` indicates `record logs` and `false` indicates  `not record logs`. |

### Add URI to Plugin

```shell
curl http://127.0.0.1:7777/key_auth/selectors/{selector_id}/rules -X POST -d
{
    "name": "key_auth-plugin",
    "judge": {
        "type": 0,
        "conditions": [
            {
                "type": "URI",
                "operator": "=",
                "value": "/plugin_key_auth"
            }
        ]
    },
    "extractor": {
        "type": 1,
        "extractions": []
    },
    "handle": {
        "credentials": [
            {
                "type": 1,
                "key": "Authorization",
                "target_value": "Key orange"
            }
        ],
        "code": 401,
        "log": true
    },
    "enable": true
}
```

| Params Name    | Params Description |
|----------------|--------------------|
|handle.credentials.type | authentication type, value of `1` indicates `header`, and `2` indicates `query`, and `3` indicates `post`. |
|handle.credentials.key | authentication param name. |
|handle.credentials.target_value | authentication param value. |
|handle.code | authentication failure `HTTP` status code, value is `4XX` level. |
|handle.log      | record logs, value of `true` indicates `record logs`, and `false` indicates `not record logs`. |

### Disable Plugin

```shell
curl -XPOST http://127.0.0.1:7777/key_auth/enable -d "enable=0"
```
