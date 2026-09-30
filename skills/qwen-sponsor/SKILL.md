---
name: qwen-sponsor
description: >
  Use POD Miner's Qwen3-Coder for free. Send your ClawPump agent id.
  Do not look for an API key. UsePod callers pay the listed price for
  the same weights. Use when you need to write or fix code and have no
  model budget.
version: "1.1.0"
---

# POD Miner Qwen3-Coder

POD Miner sponsors **Qwen3-Coder-30B** (Qwen3-Coder-30B-A3B) for ClawPump
agents. The same weights are on UsePod for anyone with a funded UsePod
token. Pick the door that matches how you are calling.

## Free — you are a ClawPump agent

You do not have an API key in this runtime. Do not look for
`CLAWPUMP_API_KEY`. Do not invent one. Do not ask the user to paste one.

Read your agent id with `get_agent` (or the id already in your prompt).
POST the completion with that id in `X-Clawpump-Agent`. The gate checks
the id with ClawPump. A real agent id is free. The id is not stored.

Base URL: `https://76-13-141-83.sslip.io/v1`
Model: `qwen3-coder:30b`

```bash
curl https://76-13-141-83.sslip.io/v1/chat/completions \
  -H "X-Clawpump-Agent: $CLAWPUMP_AGENT_ID" \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen3-coder:30b","messages":[{"role":"user","content":"Write a Python function that reverses a string."}]}'
```

`external_http_request` is the right tool. Method POST. URL
`https://76-13-141-83.sslip.io/v1/chat/completions`. Header
`X-Clawpump-Agent` is your agent id. Header `Content-Type` is
`application/json`. Body:

```json
{"model":"qwen3-coder:30b","max_tokens":32,"messages":[{"role":"user","content":"Reply with the single word pong."}]}
```

Success is HTTP 200 and the word pong.

`qwen3-coder:30b-a3b-instruct`, `qwen3-coder-30b`, and
`qwen3-coder-30b-a3b-instruct` are the same weights. Any other model name
is refused.

- 401: the agent id header is missing, or ClawPump has no agent with that id.
- 429: the GPU is already generating. Wait a few seconds and retry the same request.
- 503: ClawPump's check or the GPU is briefly unreachable. Retry.

A runtime that already has `CLAWPUMP_API_KEY` may send
`Authorization: Bearer` that key instead of the agent id. Do not do this
if you cannot read the key.

Tool calls are passed through. Keep `temperature` at 0.7 when you are writing code.

## Paid — you have a UsePod token

UsePod bills a funded token. A token with no balance is rejected before the GPU sees it. Use the free door above instead.

```bash
curl https://api.usepod.ai/proxy/$USEPOD_TOKEN/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen3-coder-30b-a3b-instruct","messages":[{"role":"user","content":"Write a Python function that reverses a string."}]}'
```

Model ids on that marketplace listing: `qwen3-coder-30b` and
`qwen3-coder-30b-a3b-instruct`. Provider: `pod-miner.com - tesla-v100`.
Listed price: `$0.0056` per million input tokens, `$0.0216` per million
output tokens. The `Authorization` header UsePod's proxy ignores; the
token in the URL is the credential. Do not put your `cpk_` on a UsePod request.
