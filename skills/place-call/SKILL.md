---
name: place-call
description: Place a single outbound AI phone call through CallForge, or check on a call in progress. Use when the user asks to call someone, dial a number, check whether calling hardware is online, or fetch a call's status, transcript, or summary.
---

# Place a phone call with CallForge

Tools live on the `callforge` MCP server. All calls are real telephone calls to real people — treat dialing as an action that needs explicit user confirmation.

## Before dialing

1. Get the destination number in **E.164** format (`+14155552671`). If the user gives a national-format number, confirm the country, convert it, and **read the full number back digit by digit** before proceeding.
2. Call `preview_phone_call` with the number and the intended objective. It validates the number, checks DNC/suppression, and reports whether the hardware (paired SIM) or cloud carriers are ready. Surface the result honestly — if the contact is suppressed or no route is ready, say so and stop.
3. **Ask the user to confirm the call** — number, objective, and route — before placing it. Never place a call the user has not confirmed.

## Placing the call

- `place_phone_call` — automatic routing (uses `auto` mode: prefers idle paired hardware for +91, otherwise cloud carrier). Honors an explicit `telephony_mode` preference (`hardware` | `cloud` | `auto`).
- `place_cloud_call` — force the cloud path (Plivo for +91, Telnyx for international) when hardware should not be used.
- `check_device_ready` — verify the paired Raspberry Pi + SIM phone bridge is online before relying on the hardware path.
- `list_cloud_phone_numbers` — show which cloud DIDs the org can call from.

A successful place call returns a call/dispatch ID — keep it for follow-up. If the tool reports a wallet/billing or license error, relay it verbatim; do not retry blindly or claim the call went through.

## After / during the call

- `get_call_status` — live state of a call or device.
- `get_cloud_dispatch_status` — carrier-side status for cloud calls by dispatch UUID.
- `get_call_transcript_and_summary` — transcript and outcome once the call ends.
- `list_call_history` — recent calls with direction/outcome filters.

## Rules

- Never claim a meeting was booked, a commitment was captured, or a result was achieved before the corresponding tool confirms it.
- Repeat callback numbers digit by digit when confirming them.
- If the objective is ambiguous, ask one clarifying question before dialing rather than guessing.
