---
name: manage-agents
description: Configure CallForge voice agents (personas) — list, create, tune prompts and voices, set the active agent, and manage knowledge docs. Use when the user asks to create or edit an AI caller, change its voice or instructions, pick which agent answers calls, or add reference knowledge.
---

# Manage CallForge voice agents

Agents (personas) are the voice + instructions the caller hears. They run on Gemini Live speech-to-speech.

## Common flows

- `list_agents` — show all personas with active status. Start here before creating or switching.
- `compile_agent_prompt` — turns business details (name, role, tone, hours, service area, things to collect, never-say list, escalation rules) into a production system prompt. Prefer this over hand-writing prompts.
- `create_or_update_agent` — create a new persona or update an existing one (display name, prompt, guardrails, inbound/outbound greetings, voice).
- `set_active_agent` — make a persona the one that answers/dials. Confirm with the user before switching the live agent.
- `delete_agent` — destructive; confirm explicitly first.
- `add_knowledge_doc` / `list_knowledge_docs` — reference material the agent can cite on calls (FAQs, pricing, policies).

## Authoring guidance

- Keep spoken greetings under ~7 seconds.
- The safety directive (`safety_directive`) is the never-break rules list — encourage users to fill it (pricing floors, no legal advice, escalation triggers).
- Available voices: Aoede, Puck, Nova, Orion, Leda, Zephyr, Kore, Charon, Fenrir, Vega, Capella, Pegasus, Ursa, Lyra, Enceladus, Polaris, Achernar, Umbriel.
- After changes, suggest a single `preview_phone_call`/`place_phone_call` test call to the user's own number so they can hear the agent before it talks to customers.

## Rules

- Never activate or delete an agent without explicit confirmation.
- When compiling prompts, keep claims truthful — the agent must not promise bookings, transfers, or capabilities the platform doesn't have (no live transfer / three-way calling).
