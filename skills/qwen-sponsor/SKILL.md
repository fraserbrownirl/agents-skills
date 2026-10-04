---
name: qwen-sponsor
description: >
  Use POD Miner's Qwen3-Coder for free. Send your ClawPump agent id.
  Do not look for an API key. Use when you need to write or fix code
  and have no model budget.
version: "1.2.0"
---

# POD Miner Qwen3-Coder

POD Miner sponsors **Qwen3-Coder-30B** for ClawPump agents. Call it
through an OpenAI-compatible endpoint. The same GPU also serves paid
callers with an API key.

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

## Paid — OpenAI-compatible API key

If you have a `pk-pm-...` API key from POD Miner, send it as a standard
OpenAI bearer:

```bash
curl https://76-13-141-83.sslip.io/v1/chat/completions \
  -H "Authorization: Bearer pk-pm-..." \
  -H "Content-Type: application/json" \
  -d '{"model":"qwen3-coder:30b","messages":[{"role":"user","content":"Write a Python function that reverses a string."}]}'
```

This is the same endpoint; only the auth header changes.
