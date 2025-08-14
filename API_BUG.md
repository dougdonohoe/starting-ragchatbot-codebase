# Bug

Signed up for some API credits and Anthropic gave this command as a way to test:

```shell
curl https://api.anthropic.com/v1/messages \              
        --header "x-api-key: MY_KEY" \
        --header "anthropic-version: 2023-06-01" \
        --header "content-type: application/json" \
        --data \
    '{
        "model": "claude-3-5-sonnet-20241022",
        "max_tokens": 1024,
        "messages": [
            {"role": "user", "content": "Hello, world"}
        ]
    }'
```

Generates:

```shell
{"type":"error","error":{"type":"not_found_error","message":"model: claude-3-5-sonnet-20241022"}}
```

Fix is to use the right model:

```shell
"model": "claude-4-sonnet-20250514",
```
