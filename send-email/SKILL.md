---
name: send-email
description: Send an email from a Lumbox inbox to any recipient. Handles threading, attachments, and returns a message ID for future replies.
version: 1.0.0
---

# Send an Email

Use this skill when an AI agent needs to send an email - a reply, a notification, a form submission, or outbound agent-to-human communication.

## When to use

- Agent needs to reply to an email it received
- Agent needs to initiate outbound contact (notify a user, file a form)
- Agent needs to forward an email

## Prerequisites

- A Lumbox inbox ID (see the `create-lumbox-inbox` skill)
- `LUMBOX_API_KEY` environment variable set
- Recipient email address

## Steps

1. **Call the send endpoint:**

   ```bash
   curl -X POST "https://api.lumbox.co/v1/inboxes/$INBOX_ID/send" \
     -H "X-API-Key: $LUMBOX_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "to": "recipient@example.com",
       "subject": "Hello from an AI agent",
       "text": "Plain text body.",
       "html": "<p>HTML body.</p>"
     }'
   ```

2. **Read the response.** You get a message ID that identifies this email for future threading:

   ```json
   {
     "id": "eml_7y3c...",
     "message_id": "<abc123@lumbox.co>",
     "sent_at": "2026-04-18T10:35:00.000Z"
   }
   ```

3. **For replies, use the reply endpoint** instead - it sets `In-Reply-To` and `References` headers correctly so the email threads in the recipient's client:

   ```bash
   curl -X POST "https://api.lumbox.co/v1/inboxes/$INBOX_ID/reply" \
     -H "X-API-Key: $LUMBOX_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{
       "reply_to_email_id": "eml_of_original",
       "text": "Thanks for the OTP."
     }'
   ```

## Tips

- Sending uses AWS SES under the hood - deliverability is high.
- For bulk sends (up to 100 recipients at once), use `/v1/send/bulk`.
- Attachments: include a `attachments` array with `filename` and base64 `content`.
- To forward: use `/v1/inboxes/:id/forward` with the email ID and new recipient.

## Related skills

- `create-lumbox-inbox` - provision the sending inbox
- `wait-for-otp` - receive the reply if the recipient sends one back
