# CallForge for Grok Bot & Cursor

Give a Grok Bot (or Cursor agent) the ability to place real AI phone calls, run outbound calling campaigns, and manage voice agents through [CallForge](https://call-agent-cloud.vercel.app) — a multi-tenant AI calling platform that routes over paired SIM hardware or cloud carriers (Plivo / Telnyx) with Gemini Live speech.

This repo is an [Agent Plugins](https://agent-plugins.org) package: a remote MCP server plus skills that teach the agent how to use it well.

## What the agent can do

| Capability | Tools |
| --- | --- |
| Place & track calls | `preview_phone_call`, `place_phone_call`, `place_cloud_call`, `get_call_status`, `get_cloud_dispatch_status`, `get_call_transcript_and_summary`, `list_call_history` |
| Campaigns | `create_campaign`, `add_campaign_lead`, `control_campaign`, `list_campaigns` |
| Voice agents | `list_agents`, `compile_agent_prompt`, `create_or_update_agent`, `set_active_agent`, `delete_agent` |
| Org setup | `check_device_ready`, `list_cloud_phone_numbers`, `get_telephony_settings`, `update_telephony_settings`, `list_contacts`, `upsert_contact`, `add_knowledge_doc`, `list_knowledge_docs`, `get_dashboard_settings`, `update_dashboard_settings` |

## Install

### Grok Bot / Cursor Marketplace

Find **CallForge** under **Settings → Plugins** (Grok Bot) or **Customize → Plugins** (Cursor), add it, then set your API key under **Plugins → Configure**.

### Manual (any MCP client)

```json
{
  "mcpServers": {
    "callforge": {
      "type": "streamable-http",
      "url": "https://call-agent-cloud.vercel.app/api/mcp",
      "headers": { "Authorization": "Bearer cfk_..." }
    }
  }
}
```

The server also supports interactive OAuth 2.1 (dynamic client registration + PKCE + consent) — clients that discover auth from the `WWW-Authenticate` challenge can connect without a static key.

## Configure

| Variable | Where to get it |
| --- | --- |
| `CALLFORGE_API_KEY` | CallForge dashboard → **Integrations → API keys** → create a `cfk_…` key. One key = one organization; revocable from the same screen. |

Never commit a real key — `mcp.json` carries only the `${CALLFORGE_API_KEY}` placeholder.

## Test locally

```bash
ln -s "$PWD" ~/.cursor/plugins/local/callforge
```

Reload Cursor, confirm the `callforge` server appears with tools, then try: *"List my CallForge agents."*

Note: Grok Bot's local-plugin path is not confirmed — verify against the current client. The reliable Grok Bot test is adding the MCP URL in chat ("Add this MCP server: …") or installing the listed plugin.

## Publish

Submit the public repo at <https://cursor.com/marketplace/publish>. Listing is manual-reviewed and requires open source. The same listing serves Cursor IDE, CLI, and Grok Bot — there is no separate Grok Bot submission form.

## License

MIT — see [LICENSE](LICENSE).
