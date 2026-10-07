# CTL alert test — field mode

The Round 2 diagnostic flow (Ranjana's `voice-copilot-v2`), adapted to be sent to real operators over WhatsApp.

## Before sending any link

Edit the `FIELD` block in `index.html` (search for `FIELD MODE`):

- `reportTo` — WhatsApp number that receives "मदद चाहिए" messages (digits only, e.g. `919812345678`)
- `supervisor.name`, `supervisor.tel` — who the call button dials. If empty, it dials `reportTo`. It never dials the old prototype number.
- `ntfy` — the live event feed topic (already set to a random value)

## Links

One link per operator per variant:

```
https://<user>.github.io/dp-ctl-alert/?o=<operator>&v=<variant>&p=<plant>
```

- `o` — operator name, shows up in every event
- `v` — message variant you sent (A / B / C)
- `p` — plant name shown on the issue card (optional)
- `f=B` — Low DO flow instead of the pump leak (optional)

Opening the link is the "tapped" signal.

## Watching runs live

Install the ntfy app (Android / iOS) and subscribe to the topic in `FIELD.ntfy`, or open `https://ntfy.sh/<topic>` in a browser.

| Event | Means |
|---|---|
| OPENED | operator tapped the link (with time) |
| STEP | reached a step (silent) |
| CAUSE | picked a root cause |
| HELP | asked the engineer for help (also opens WhatsApp to `reportTo`) |
| CALL | pressed call supervisor |
| AWAY / BACK | left the page and returned |
| CLOSED | closed the page before finishing — the drop-out step |
| DONE / ENDED | full summary: time, cause, step-by-step dwell |

The ntfy topic is public to anyone who knows its name. Events carry the operator name you put in `o` and nothing else.

## What changed from the prototype

Removed every invented plant fact a real operator could act on: live readings (pressure, DO), plant name and capacity, past incidents, maintenance dates, the "memory" hypothesis card, and plant-specific dosing / backwash / HMI numbers (those now route to the engineer). The step questions and their advice are unchanged.
