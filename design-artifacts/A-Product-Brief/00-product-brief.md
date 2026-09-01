# Product Brief: University AI Compute Management Platform

> The strategic foundation — why this product exists, who it serves, and what success looks like.

**Created:** 2026-08-31
**Phase:** 1 — Product Brief
**Agent:** Saga (Analyst)

---

## What Belongs Here

The Product Brief answers five strategic questions:

1. **Why** does this product exist? (Vision & business goals)
2. **Who** is it for? (Target users and their context)
3. **What** does it need to do? (Core capabilities)
4. **How** will we know it works? (Success metrics)
5. **What** are the constraints? (Platform requirements, tech stack)

Everything downstream — trigger maps, scenarios, page specs, design system — traces back to decisions made here. This is the North Star.

**Learn more:**
- WDS Course Module 04: Product Brief — Your Strategic Foundation
- WDS Course Module 05: Platform Requirements

---

## For Agents

**Workflow:** `skill:wds-1-project-brief`
**Agent trigger:** `PB` (Saga)
**Templates:** `./resources/wds-1-project-brief/templates/`

**Before writing anything in this folder:**
1. Load the workflow and follow its steps
2. Use Dialog mode for discovery — ask questions, don't assume
3. Read existing materials if the user has them (check `wds-project-outline.yaml`)

**File naming:** Number all documents with a two-digit prefix: `01-product-brief.md`, `02-content-language.md`, etc. Platform Requirements is always last — it summarizes technical decisions that emerge from the strategic documents above. Update the Documents table below as each file is created.

**Harm:** Producing a brief from assumptions instead of conversation. A brief that doesn't reflect the user's actual goals forces every later phase to build on a wrong foundation.

**Help:** Asking the right questions, listening deeply, and documenting what the user actually said. A good brief makes every later decision easier.

---

## Existing Materials (from Phase 0)

- **SPEC-1** (formal specification document)
- **Student Project Brief**

Key capabilities/rules captured at intake — Saga should read these in full during Phase 1, not just this summary:
- Requests evaluated immediately, deterministic outcomes restricted to exactly four statuses: `Accepted`, `Denied: Not Authorized`, `Denied: Resource Unavailable`, `Denied: Reservation Conflict`. No queuing, holding, or automatic retries in this iteration.
- Manages heterogeneous local GPU fleet (mostly 24GB VRAM machines, one dual 48GB GPU machine) over Kubernetes (device plugins + Kubeflow Training Operator).
- Access control governed by academic context (course, group, privilege level) and time-window restrictions for heavy workload types (e.g. training jobs).
- Scheduled academic class reservations take full precedence — reject conflicting general-use requests.
- System monitors/reports usage at token level (prompt/generation tokens) and tracks infra status (available, reserved, busy, idle, failing).

## Documents

_This section will be updated as documents are created during Phase 1._

| # | Document | Status |
|---|----------|--------|
| 01 | Product Brief | Complete — [project-brief.md](project-brief.md) (renamed from `01-product-brief.md` to match task-spec2's required path) |
| 02 | Platform Requirements | Not started |

---

_Created using Whiteport Design Studio (WDS) methodology_
