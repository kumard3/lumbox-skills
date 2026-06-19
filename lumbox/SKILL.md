---
name: lumbox
description: Give an AI agent its own email — create inboxes, send and receive email, wait for OTP codes, all self-service with no human signup. Use when an agent needs an email address to sign up for services, receive verification codes, or manage correspondence.
version: 1.0.0
---

# Lumbox — email for AI agents

Lumbox gives agents real email inboxes via CLI, REST API, or MCP. An agent can provision
itself with no human account creation.

## Self-signup (no human needed)

CLI (requires Node 18+):

    npx lumbox signup --source <your-harness-name>

This creates an org + API key and saves it to `~/.lumbox/config.json`. Or via curl:

    curl -X POST https://api.lumbox.co/v1/orgs \
      -H 'Content-Type: application/json' \
      -d '{"name": "my-agent-org", "source": "skill"}'

Save the returned `api_key` (shown only once). Export it as `LUMBOX_API_KEY`.
Free tier: 3 inboxes, 500 inbound + 100 outbound emails/month. Self-signup is rate-limited
to 3 orgs/hour per IP.

## Core flows (CLI)

    npx lumbox inbox create --name assistant
    npx lumbox send --inbox <id> --to a@b.com --subject "Hi" --text "Hello"
    npx lumbox wait-otp --inbox <id> --timeout 120

`inbox create` returns `{id, address}`; `wait-otp` blocks until an OTP arrives and prints
the parsed code. All CLI output is JSON.

## Core flows (REST)

    POST /v1/inboxes                  create inbox
    GET  /v1/inboxes/:id/wait         long-poll for next email (120s)
    GET  /v1/inboxes/:id/otp          long-poll for OTP, returns parsed code
    POST /v1/inboxes/:id/send         send email
    POST /v1/inboxes/:id/reply        reply in thread

Auth: `Authorization: Bearer $LUMBOX_API_KEY`. Base URL: `https://api.lumbox.co`.

## Full toolset (MCP, 87 tools)

    claude mcp add lumbox -- npx @lumbox/mcp-server

or HTTP MCP at `https://mcp.lumbox.co` with the same Bearer key. Includes browser
automation, credential vault, webhooks, custom domains, contact lists.

Docs: https://docs.lumbox.co • Agent guide: https://docs.lumbox.co/docs/agent-signup
