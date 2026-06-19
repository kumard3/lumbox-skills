---
name: lumbox-sdk
description: Use the Lumbox SDK to give AI agents email - create inboxes, long-poll for OTPs, and send/reply/forward from TypeScript or Python. Use when integrating Lumbox into agent code with the official npm or PyPI package instead of raw HTTP.
---

# Lumbox SDK

Email infrastructure for AI agents. One call creates an agent-owned inbox; one long-poll call returns the OTP.

- API base: `https://api.lumbox.co/v1`
- OpenAPI: https://lumbox.co/openapi.json
- Auth: `X-API-Key: ak_...` (create at https://app.lumbox.co/settings/api-keys)

## Install

```bash
npm install lumbox     # TypeScript / Node
pip install lumbox     # Python
```

## TypeScript

```ts
import { Lumbox } from "lumbox";
const lumbox = new Lumbox({ apiKey: process.env.LUMBOX_API_KEY });

const inbox = await lumbox.inboxes.create({ name: "github-bot" });
// ...trigger a signup that emails inbox.address...
const otp = await lumbox.inboxes.otp(inbox.id, { timeout: 60 });
console.log(otp.code); // "847291"

await lumbox.inboxes.send(inbox.id, {
  to: "user@example.com",
  subject: "Hello",
  text: "Hi there",
});
```

## Python

```python
from lumbox import Lumbox
lumbox = Lumbox(api_key=os.environ["LUMBOX_API_KEY"])

inbox = lumbox.inboxes.create(name="github-bot")
otp = lumbox.inboxes.otp(inbox.id, timeout=60)
print(otp.code)
```

## Raw HTTP (any language)

```bash
curl -X POST https://api.lumbox.co/v1/inboxes \
  -H "X-API-Key: $LUMBOX_API_KEY" -H "Content-Type: application/json" \
  -d '{"name":"github-bot"}'
```

## References

- API reference: https://docs.lumbox.co/docs/api-reference
- SDK quickstart: https://docs.lumbox.co/docs/sdk-quickstart
- Errors are JSON `{ "error": "..." }`; rate limit 120 req/min with `X-RateLimit-*` headers.
