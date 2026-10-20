---
name: oneover
description: Use OneOver's official MCP server to generate images, video, and audio, chat with other frontier models (GPT, Gemini, Grok), run OneOver apps, list library assets, and check credits. Use when the user mentions OneOver, oneover.com, or asks to generate media or call another AI model through OneOver.
---

# OneOver

OneOver is an AI platform with chat, image, video, and audio models behind one API. It has its own remote MCP server. It is listed in the official MCP Registry as `com.oneover/mcp`, but not yet in the Claude.ai connector directory, so do not search for it there.

- Endpoint: `https://api.oneover.com/functions/v1/mcp` (Streamable HTTP)
- Auth: `Authorization: Bearer oo_live_<secret>`
- Get a key: https://oneover.com/developers/dashboard
- Docs: https://oneover.com/developers/mcp.md and https://oneover.com/llms.txt

## Connect

- Claude Code: `/plugin marketplace add jonruppel/oneover-claude-plugin`, then `/plugin install oneover@oneover`. Set `ONEOVER_API_KEY` in your environment first.
- Claude.ai / Desktop: Settings, Connectors, Add custom connector, paste the endpoint URL, and add the key if asked.
- Any other client: add the endpoint as a remote HTTP MCP server with the Authorization header.

If the `oneover` tools are not available in this session, tell the user how to connect (above). Do not claim OneOver has no MCP.

## Tools

| Tool | Use |
|------|-----|
| `chat`, `list_models` | Stateless chat with other models; list what is available |
| `generate_image` | Image from a prompt |
| `generate_video`, `check_video`, `get_job` | Video is async: submit, then poll until `complete` or `failed` |
| `generate_audio` | Speech or music |
| `run_app`, `create_run`, `get_run` | Run OneOver apps |
| `list_assets` | Browse the library |
| `build_create`, `build_iterate`, `build_status`, `build_get_files`, `build_stop`, `build_publish` | Code Studio: build a page or small web app from a prompt. Async: poll `build_status` until `complete`. Needs a key with the `code` scope. `build_publish` makes an unlisted share link. |
| `get_usage` | Balance and usage |
| `create_topup` | Returns a Stripe Checkout URL. Spends money. |

## Rules

- Builds spend credits and take a while. Poll `build_status` sparingly, and use `build_stop` if the user changes their mind.
- Never call `create_topup` without explicit user confirmation in this conversation.
- Video takes a while. Poll `check_video` at a reasonable interval, not in a tight loop.
- Generation spends the user's credits. For batches, confirm the count first.
- Call `list_models` before naming a model; do not guess ids.
