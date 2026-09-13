---
name: run-campaign
description: Create and control outbound AI calling campaigns in CallForge — add leads, start/pause/stop dialing, and read campaign statistics. Use when the user wants to call a list of contacts, run an outreach campaign, or check campaign progress.
---

# Run a CallForge campaign

A campaign dials a list of leads with a shared objective, one call at a time on hardware (paired SIM) or concurrently on cloud carriers.

## Setup flow

1. `create_campaign` — needs a name, the campaign objective, and behavioral instructions for the voice agent. Confirm the objective wording with the user before creating; the prompt is what the agent actually says on calls.
2. `add_campaign_lead` — once per lead, with the phone number (E.164) plus any background context the agent should use (name, company, prior touchpoints). Batch-add every lead before starting.
3. `control_campaign` with `start` to begin dialing. Confirm with the user before starting — this places real calls.
4. `list_campaigns` — live stats: pending leads, in-flight dials, completed, stopped.
5. `control_campaign` with `pause` / `stop` — pause keeps pending leads, stop cancels them.

## Useful supporting tools

- `list_contacts` / `upsert_contact` — find or maintain the org's contact records for lead lists.
- `get_telephony_settings` / `update_telephony_settings` — inspect or change routing mode (`hardware`, `cloud`, `auto`), carrier preference, and cloud concurrency.
- `list_call_history` / `get_call_transcript_and_summary` — per-lead outcomes once calls complete.

## Rules

- Confirm lead count and objective before `start`. Summarize: "N leads, objective X, route Y — start?"
- Pacing on hardware is sequential (one paired SIM = one call at a time). Set expectations: large lead lists take a while on `hardware` mode.
- Report failures faithfully — suppressed numbers, wallet depletion, and carrier errors appear in campaign stats and call history; surface them rather than smoothing them over.
- Do not edit a running campaign's objective by recreating it — report status and let the user decide.
