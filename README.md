# Agent Payment Safety + Recommend API (x402, Base mainnet)

Micropayment API for AI agents — pay per call in USDC, **no API keys, no signup**. Powered by [x402](https://www.x402.org).

| Endpoint | Price | What it does |
|---|---|---|
| `GET /v1/safety/check?target=<url>` | **$0.002** | Deterministic payment-safety scan of a URL: x402 manifest check, advisory allow/warn/block verdict, signed receipt |
| `GET /v1/recommend?category=<cat>` | **$0.005** | Ranked provider recommendations across 8 categories (search, llm, vision, tts, payments, data, storage, compute) |
| `GET /health` | free | Liveness |
| `GET /v1/verify?receipt=<id>` | free | Verify a receipt |
| `GET /openapi.json` | free | OpenAPI 3.0 with x-402 metadata |

Base URL: https://places-wayne-experiment-facial.trycloudflare.com

## How to call (x402 flow)

```bash
# 1. Call without payment header -> HTTP 402 with exact price
curl "https://places-wayne-experiment-facial.trycloudflare.com/v1/safety/check?target=https://example.com"
# -> JSON: x402Version, accepts[] with network=base, maxAmountRequired=2000 atomic USDC (= $0.002)

# 2. Pay with any x402 client, retry with X-PAYMENT header
```

Python (official `x402` client):

```python
from x402.clients.requests import enable_payments
s = enable_payments(private_key="0xYOUR_KEY")  # pays ~$0.002 USDC per call, Base mainnet
r = s.get("https://places-wayne-experiment-facial.trycloudflare.com/v1/safety/check", params={"target": "https://example.com"})
print(r.status_code, r.json())
```

## x402 ecosystem

- Network: **base** (mainnet), asset **USDC** `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`
- Facilitator: `https://x402.org/facilitator`
- payTo: `0x35d78b68740AFcDE302978815C9033556b6eadB4`
- Indexed on **x402dash** (verified, liveness-monitored)
- OpenAPI: https://places-wayne-experiment-facial.trycloudflare.com/openapi.json
