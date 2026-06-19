---
name: wait-for-otp
description: Block until an OTP code arrives at a Lumbox inbox and return the parsed code. Avoids polling loops — the request long-polls the server.
version: 1.0.0
---

# Wait for OTP

Use this skill when an AI agent has triggered an OTP-bearing email (e.g. by submitting a sign-up or login form) and needs to read the code before it expires.

## When to use

- Agent just requested an OTP via a sign-up or 2FA flow
- Agent needs to extract a numeric or alphanumeric code from incoming email
- You want to avoid writing polling logic

## Prerequisites

- A Lumbox inbox ID (see the `create-lumbox-inbox` skill)
- The inbox address must have been provided to the service that sends the OTP
- `LUMBOX_API_KEY` environment variable set

## Steps

1. **Call the long-poll endpoint.** Lumbox holds the connection open until an OTP arrives, then returns it:

   ```bash
   curl "https://api.lumbox.co/v1/inboxes/$INBOX_ID/otp?timeout=60" \
     -H "X-API-Key: $LUMBOX_API_KEY"
   ```

   The `timeout` query param controls max wait time in seconds (default 30, max 120).

2. **Read the response.** The API returns the extracted code and the email it came from:

   ```json
   {
     "otp": "847291",
     "email_id": "eml_9a2b...",
     "received_at": "2026-04-18T10:30:22.123Z"
   }
   ```

3. **Use the code.** Submit `otp` into the form/API field that's waiting for it.

## Tips

- If `timeout` elapses with no email, the endpoint returns `204 No Content`. Re-call to extend the wait.
- Lumbox extracts codes via regex tuned for 4-8 digit numeric, alphanumeric, and format-with-dash codes.
- For magic links instead of OTPs, call `/v1/inboxes/:id/wait` and read the parsed links.
- Backup codes and expiry timestamps are also extracted into the email record.

## Related skills

- `create-lumbox-inbox` — provision the inbox first
- `send-email` — reply to the OTP sender or confirm receipt
