## URI Rewrite Plugin

Used to rewrite `URI` information upstream of the request.

### Enable Plugin

```shell
curl -XPOST http://127.0.0.1:7777/rewrite/enable -d "enable=1"
```

### Add Selectors to Plugin

```shell
curl http://127.0.0.1:7777/rewrite/selectors -X POST -d '
{
    "name": "rewrite-selectors",
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
curl http://127.0.0.1:7777/rewrite/selectors/{selector_id}/rules -X POST -d
{
    "name": "rewrite-plugin",
    "judge": {
        "type": 0,
        "conditions": [
            {
                "type": "URI",
                "operator": "=",
                "value": "/plugin_rewrite"
            }
        ]
    },
    "extractor": {
        "type": 1,
        "extractions": []
    },
    "handle": {
        "uri_tmpl": "/plugin_rewrite",
        "log": true
    },
    "enable": true
}
```

| Params Name    | Params Description |
|----------------|--------------------|
|handle.uri_tmpl | upstream new uri，value of `normal` indicates `current value`, and `extraction` indicates `variable extraction`.|
|handle.log      | record logs, value of `true` indicates `record logs`, and `false` indicates `not record logs`. |

### Disable Plugin

```shell
curl -XPOST http://127.0.0.1:7777/rewrite/enable -d "enable=0"
```
