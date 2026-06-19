---
name: create-lumbox-inbox
description: Create a fresh Lumbox email inbox for an AI agent. Returns a real email address the agent can receive OTPs, magic links, and messages at.
version: 1.0.0
---

# Create a Lumbox Inbox

Use this skill when an AI agent needs a real, working email address - for example, to sign up for a third-party service, receive an OTP, or communicate with humans.

## When to use

- Agent needs to sign up for a SaaS that requires email verification
- Agent needs to receive an OTP or magic link
- Each workflow/session needs an isolated inbox (no cross-contamination)

## Prerequisites

- A Lumbox API key. Create one at https://app.lumbox.co/settings/api-keys.
- Set the key in the `LUMBOX_API_KEY` environment variable.

## Steps

1. **Call the API** with POST `https://api.lumbox.co/v1/inboxes`:

   ```bash
   curl -X POST https://api.lumbox.co/v1/inboxes \
     -H "X-API-Key: $LUMBOX_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"name": "github-signup-bot"}'
   ```

2. **Read the response.** Extract `address` (the real email address) and `id` (the inbox identifier):

   ```json
   {
     "id": "inb_8x2f9a...",
     "address": "github-signup-bot@lumbox.co",
     "imap_password": "..."
   }
   ```

3. **Use the address.** Provide `address` as the email during sign-up or anywhere an email is needed. Emails sent to this address arrive in the inbox within seconds.

## Tips

- Use descriptive names (`name`) so you can find the inbox later in the dashboard.
- To receive email at a custom domain (e.g. `agent@mycorp.com`), configure the domain first via POST `/v1/domains`.
- Free tier: 3 inboxes, 500 emails/month. Upgrade at https://lumbox.co/pricing.

## Related skills

- `wait-for-otp` - block until an OTP code arrives
- `send-email` - send email from an inbox
