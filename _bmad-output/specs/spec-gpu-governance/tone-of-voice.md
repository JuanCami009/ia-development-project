# Tone of Voice

For UI microcopy and system messages — every denial, success message, button label, and empty state.

## Tone attributes

1. **Precise & Explicit** — every message states the exact cause. Never a generic "error occurred": naming the rule that fired is the product's whole differentiator.
2. **Calm & Non-punitive** — a denial is a fact of system state, not a scolding. No blame language, no exclamation points on failures.
3. **Direct & Efficient** — users are often mid-task under time pressure (class starting in 10 minutes, assignment due). No pleasantries padding the message.
4. **Quietly Authoritative** — confident and matter-of-fact, not bureaucratic/legalese, not overly formal.

## Worked examples

| Situation | ✅ Use | ❌ Avoid |
|---|---|---|
| Denial (reservation conflict) | "Denied — Reservation Conflict. This GPU is reserved for [Course Name] from 14:00–16:00." | "Error: Request denied" |
| Denial (authorization) | "Denied — Not Authorized. Your group doesn't have privilege level for this workload type." | "Access denied" |
| Button text | "Request GPU" / "Reserve Window" | "Submit" |
| Success | "Accepted. Running on gpu-node-04." | "Operation completed successfully" |
| Empty state | "No active requests right now." | "No results found" |

## Do

- Name the specific rule/cause behind every denial (Not Authorized / Resource Unavailable / Reservation Conflict) with concrete detail — which course, which window, which quota.
- Keep success/status messages short and factual: what happened, where it's running.
- Write for someone under time pressure — assume they'll skim.

## Don't

- Use generic error codes or vague failure language without a named cause.
- Add blame-toned or apologetic language to denials — it's a rule firing, not a mistake.
- Pad messages with pleasantries or exclamation marks.
