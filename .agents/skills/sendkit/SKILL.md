---
name: sendkit
description: Use Sendkit to send Telegram messages from agents through the Sendkit MCP tool or CLI fallback. Use when a user asks to send a Telegram message, use Sendkit, interact with the Sendkit toolset, verify Sendkit manually, or choose between Sendkit MCP and CLI workflows.
---

# Sendkit

Sendkit lets agents send Telegram messages. There are two paths:

- **MCP tool** — preferred, faster, no setup per message
- **CLI fallback** — works when MCP is unavailable or for verification

## MCP tool

The MCP server is published on npm as [`@lexand/sendkit-mcp`](https://www.npmjs.com/package/@lexand/sendkit-mcp). Configure it in your MCP client:

```json
{
  "mcp": {
    "sendkit": {
      "type": "local",
      "command": ["npx", "-y", "@lexand/sendkit-mcp"],
      "environment": {
        "TELEGRAM_BOT_TOKEN": "<your-bot-token>"
      }
    }
  }
}
```

Once configured, the `telegram` tool becomes available in your toolset. Use it directly:

```
telegram({ chatId: "<chatId>", message: "<text>" })
```

If the MCP tool fails with a token error, fall back to the CLI.

## CLI fallback

The CLI is published on npm as [`@lexand/sendkit`](https://www.npmjs.com/package/@lexand/sendkit). Install globally:

```bash
npm i -g @lexand/sendkit
```

### One-time setup

```bash
sendkit init --telegram-bot-token <your-bot-token>
```

This stores the token at `~/.config/sendkit/config.json`.

### Sending a message

```bash
sendkit telegram "<chatId>" "<message>"
```

## Verification

To verify Sendkit is working, send a test message. If the user doesn't know their chat ID, guide them to:

1. Message their bot on Telegram
2. Open `https://api.telegram.org/bot<bot-token>/getUpdates`
3. Find `"chat":{"id":<number>}` in the response
