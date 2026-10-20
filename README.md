# OneOver for Claude Code, Cursor, and other MCP clients

[OneOver](https://oneover.com) is a remote [MCP](https://modelcontextprotocol.io) server that puts chat, image, video, and audio models from several vendors behind one API key. This repo is the official Claude Code plugin and the setup recipes for other clients.

- **Endpoint:** `https://api.oneover.com/functions/v1/mcp` (Streamable HTTP, nothing to install)
- **Auth:** `Authorization: Bearer oo_live_<secret>`. Mint a key at https://oneover.com/developers/dashboard
- **Inspect without a key:** `initialize` and `tools/list` work unauthenticated. `tools/call` needs a key.
- **Server card:** https://oneover.com/.well-known/mcp/server-card.json

## Claude Code

Plugin (adds the server and a skill that tells Claude when to use it):

```
export ONEOVER_API_KEY=oo_live_...
/plugin marketplace add jonruppel/oneover-claude-plugin
/plugin install oneover@oneover
```

Or just the server:

```
claude mcp add --transport http oneover https://api.oneover.com/functions/v1/mcp --header "Authorization: Bearer oo_live_YOUR_KEY"
```

## Cursor

`.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "oneover": {
      "url": "https://api.oneover.com/functions/v1/mcp",
      "headers": { "Authorization": "Bearer oo_live_YOUR_KEY" }
    }
  }
}
```

## Claude Desktop and Claude.ai

Settings, Connectors, Add custom connector. Paste the endpoint URL and add the Authorization header if asked. For the desktop config file, use the same JSON as Cursor in `claude_desktop_config.json`.

## Tools

| Tool | What it does |
|------|--------------|
| `chat`, `list_models` | Stateless chat with any model your key can use; list the catalog |
| `generate_image` | Image from a prompt |
| `generate_video`, `check_video`, `get_job` | Async video. Submit, then poll until `complete` or `failed` |
| `generate_audio` | Speech or music |
| `run_app`, `create_run`, `get_run` | Run OneOver apps; durable runs that survive a disconnect |
| `list_assets` | Browse the library |
| `build_create`, `build_iterate`, `build_status`, `build_get_files`, `build_stop`, `build_publish` | Code Studio builds. Async. Needs the `code` scope. |
| `get_usage` | Balance and usage |
| `create_topup` | Stripe Checkout URL. Spends money, so confirm first |

## Examples

Ask Claude Code:

- "Use OneOver to ask Gemini to review this diff and tell me where it disagrees with your review."
- "Generate a 1:1 product image of a ceramic mug on a walnut desk with OneOver."
- "Use `list_models`, then run the same prompt on two text models and compare."

## Costs and limits

Calls spend credits from the key's organization. Image and video cost more than chat. Rate limit is 60 requests per minute per key, 300 for elevated keys. If you only need one vendor and already hold that vendor's key, call the vendor directly.

## Docs

[MCP docs](https://oneover.com/developers/mcp) · [Guide: GPT and Gemini from Claude Code](https://oneover.com/guides/gpt-and-gemini-in-claude-code) · [OpenAPI](https://oneover.com/openapi.json) · [llms.txt](https://oneover.com/llms.txt)

License: MIT
