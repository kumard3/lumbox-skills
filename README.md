# Lumbox Skills

Official agent skills for [Lumbox](https://lumbox.co) - email infrastructure for AI agents. Give an agent its own inbox, extract OTPs and verification links to JSON, and send/reply/forward over one REST API and a hosted MCP server.

## Install a skill

```bash
npx skills add kumard3/lumbox-skills          # all skills
npx skills add kumard3/lumbox-skills/lumbox    # one skill
```

## Skills

| Skill | What it does |
|-------|--------------|
| [`lumbox`](./lumbox) | Give an agent its own email: create inboxes, send/receive, wait for OTPs |
| [`lumbox-mcp`](./lumbox-mcp) | Connect the Lumbox MCP server (Claude Code, Cursor) |
| [`lumbox-sdk`](./lumbox-sdk) | Use the TypeScript / Python SDK |
| [`create-lumbox-inbox`](./create-lumbox-inbox) | Provision a fresh agent inbox |
| [`wait-for-otp`](./wait-for-otp) | Long-poll for a verification code |
| [`send-email`](./send-email) | Send from an inbox with threading |
| [`email-for-ai-agents`](./email-for-ai-agents) | When/why agents need email; category map |
| [`agent-email-patterns`](./agent-email-patterns) | Signup-verify, 2FA, multi-agent, security |

## Links

- Website: https://lumbox.co
- Docs: https://docs.lumbox.co
- OpenAPI: https://lumbox.co/openapi.json
- Hosted MCP: https://mcp.lumbox.co/mcp
- npm SDK: https://www.npmjs.com/package/lumbox · Python: https://pypi.org/project/lumbox
- Free tier (no card): https://app.lumbox.co/signup

## License

MIT
