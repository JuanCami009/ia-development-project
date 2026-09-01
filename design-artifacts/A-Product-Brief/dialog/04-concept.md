# Step 7a: Product Concept

**Completed:** 2026-08-31
**Session:** 1

---

## Opening Question

**Agent asked:**
"We've established vision, positioning, target users. Now the structural idea — the founding principle that makes this actually work differently. From the materials, it smells like: immediate synchronous evaluation gate, not a queue — is that the real organizing principle, or is there a different core mental model?"

**User's initial response:**
Confirmed directly: "Immediate Synchronous Evaluation Gate, Not a Queue" — with full mental-model breakdown (evaluation trigger, deterministic four-outcome resolution, no retries/queueing, reservations as state entries not a separate execution path).

---

## Key Exchanges

**Agent:** What's the rationale — why this approach over a queue?
**User:** Queuing in a scarce heterogeneous GPU environment creates hidden bottlenecks and false expectations. An immediate gate gives instant determinism — users know right now whether their workload runs or exactly why not — which is what makes denials 100% explainable to students and the decision trail auditable to administration. Reservations act as state entries that immediately alter evaluation conditions, so scheduled class windows are protected automatically without human intervention.

---

## Reflection Checkpoint

**Agent's synthesis:**
"Core Structural Idea: immediate synchronous evaluation gate, not a queue — every request resolves once, on the spot, to exactly one of four terminal outcomes (Accepted / Denied: Not Authorized / Denied: Resource Unavailable / Denied: Reservation Conflict), no holding, no auto-retry. Why: queuing would just formalize the opacity/false-hope problem that caused this project to exist; an immediate gate replaces 'maybe later' with an instant, explainable, auditable answer. Concrete example: reservations aren't a separate execution path — they're state entries that, once active, force Denied: Reservation Conflict on any colliding general-use request automatically."

**User response:**
- [x] Confirmed
- [ ] Corrected

**Corrections (if any):**
None — user validated both the structural idea and the rationale as "100% accurate" / "fully validated," adding supporting detail rather than correcting.

---

## Final Concept Documentation

**Core Structural Idea:**
Immediate synchronous evaluation gate, not a queue. Every request (access/usage, job execution, reservation) is evaluated once, on receipt, against current system state, academic context, active reservations, and capacity.

**Implementation Principle:**
Deterministic terminal resolution — exactly one of four outcomes per request: Accepted, Denied: Not Authorized, Denied: Resource Unavailable, Denied: Reservation Conflict. No holding, no queueing, no automatic retries in this iteration. Reservations are not a separate execution model — they are state entries; once active, they modify what the gate evaluates, automatically producing Denied: Reservation Conflict for any colliding general-use request.

**Rationale:**
Queuing in a scarce, heterogeneous GPU environment would formalize the exact opacity and false-hope problem that made informal coordination fail. An immediate gate trades "maybe later" for an instant, explainable answer — 100% explainable denial reasons for students, an auditable decision trail for administration, and automatic (not manual) protection of scheduled class windows.

**Concrete Example:**
A teacher's class reservation is active as a state entry. A student's ad-hoc job submitted during that window collides with it and is evaluated against that state — the gate returns `Denied: Reservation Conflict` immediately, with no human intervention needed to protect the class.

**Features That Stem From This Concept:**
- Synchronous request evaluation endpoint (no background job/queue worker for admission decisions)
- Reservation-as-state-entry model (reservations write to the same state the gate reads, rather than routing through a separate scheduler)
- Four-outcome response contract surfaced directly to students (exact denial cause, not a generic failure)
- No retry/backoff logic needed in this iteration — a denied request is final until resubmitted

---

**Documented in:** `wds-project-outline.yaml` → `product_concept`
