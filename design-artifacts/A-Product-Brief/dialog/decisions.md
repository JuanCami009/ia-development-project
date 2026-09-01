# Key Decisions Log

**Project:** University AI Compute Management Platform
**Format:** Append-only decision log

---

## Decision 1: Client structure — sole builder, three satisfaction groups

**Date:** 2026-08-31
**Step:** 01a — Client Profile
**Session:** 1

**Context:**
Needed to know who actually decides on this design vs. who must merely be satisfied by the result.

**What was decided:**
JC is sole builder/technical contact with full decision authority for the spec/design phase. Academic Administration, Teachers, and Lab Operators are stakeholders the *design* must satisfy, but none of them co-decide the design itself.

**Why:**
Small internal/course project, no agency or committee structure — but the product governs a shared scarce resource across groups with real (if informal) authority, so their needs still constrain what "good" looks like.

**Impact:**
Downstream brief work can move fast (no approval-chain friction), but success criteria and constraints must be evidence-based against each stakeholder's stated need — not just JC's preference.

**Alternatives considered:**
- Treat Academic Administration as formal approver — not chosen, no evidence of a formal sign-off gate beyond the academic evaluation cycle

**Documented in:** `dialog/client-profile.md`

---

## Decision 2: Internal driver = breakdown of informal coordination

**Date:** 2026-08-31
**Step:** 01a — Client Profile
**Session:** 1

**Context:**
Establishing why this project exists now, beyond "it's a course assignment."

**What was decided:**
Root trigger is informal GPU coordination breaking down under a heterogeneous fleet (24GB machines + one dual-48GB machine) — causing resource conflicts, teacher uncertainty before scheduled classes, and student friction from opaque rejections.

**Why:**
JC named this directly, citing source material (SPEC-1 / Student Project Brief).

**Impact:**
Vision and success-criteria steps should anchor on eliminating these three specific pain points (conflicts, teacher uncertainty, opaque rejections) rather than generic "better GPU scheduling" framing.

**Alternatives considered:**
- N/A — directly stated, not inferred

**Documented in:** `dialog/client-profile.md`

---

## Open Item: Source documents referenced but not yet shared in full

**Date:** 2026-08-31
**Step:** 01a — Client Profile
**Session:** 1

JC's answers carry citation markers (e.g. `[cite: 2, 3]`) pointing to SPEC-1 and the Student Project Brief. Only a summary of these exists in `wds-project-outline.yaml`. Flag for later: if exact requirement wording matters (e.g. the four deterministic statuses, time-window rules), pull the source documents verbatim rather than relying on paraphrase.

---

## Decision 3: Business model = internal/non-commercial (neither B2B nor B2C)

**Date:** 2026-08-31
**Step:** 05 — Business Model
**Session:** 1

**Context:**
Needed to establish who pays and who uses, and whether any commercial buyer/seller relationship exists.

**What was decided:**
Purely internal institutional infrastructure. No B2B, no B2C. Funded entirely by university/department IT budget. Owned by Academic Administration + Lab Operations. Quotas/permissions (by course/group/privilege) act as the allocation currency instead of money. Token tracking exists strictly for usage reporting, fair-share governance, and capacity planning — never billing.

**Why:**
Confirmed directly and unambiguously by JC, consistent with the "no financial chargebacks" boundary already stated in Phase 0 materials and the Vision step.

**Impact:**
- No pricing/procurement/payment UI in scope, ever.
- Step 06 (Business Customers) skipped — routes straight to Step 07 (Target Users), since there is no B2B buyer to profile.
- Success and stakeholder framing throughout the brief must stay operational, not financial.

**Alternatives considered:**
- B2B (department "pays" for the service internally) — rejected, no actual transactional/procurement relationship exists, just budget ownership
- Freemium/quota-as-paid-tier model — rejected, quotas are governance-based, not monetizable

**Documented in:** `01-product-brief.md` → Business Model

---

## Decision 4: Primary success metric = zero class interruptions

**Date:** 2026-08-31
**Step:** 08 — Success Criteria
**Session:** 1

**Context:**
Multiple SMART candidate metrics were surfaced (zero interruptions, 100% explainability, sub-second latency, 100% decision consistency, zero manual escalations). Needed one prioritized above the rest for trade-off decisions later.

**What was decided:**
Zero class interruptions per semester is the single primary metric. All others (explainability, latency, consistency, escalations) are secondary/supporting.

**Why:**
An interrupted class is an immediate, unrecoverable trust-breaker with Teachers and Academic Administration — worse than a denied student request, which can at least be explained after the fact.

**Impact:**
When later design/architecture trade-offs conflict (e.g. simplicity vs. reservation-conflict robustness), protecting scheduled class windows wins by default.

**Alternatives considered:**
- 100% explainability rate as primary — rejected, important for trust but not as unrecoverable a failure as a live class losing GPU capacity

**Documented in:** `01-product-brief.md` → Success Criteria

---

## Decision 5: Unfair advantage = paradigm gap, not a feature gap

**Date:** 2026-08-31
**Step:** 09 — Competitive Landscape
**Session:** 1

**Context:**
Reality-checked whether Slurm/PBS (the closest real alternative) could close the gap by simply adding course/group awareness.

**What was decided:**
No — the gap is structural. Slurm is built for long-running HPC batch scheduling/node reservations; this platform is an immediate, zero-queue, deterministic evaluation gate for interactive LLM workloads. Slurm also lacks native prompt/generation token tracking and human-explainable denial feedback.

**Why:**
Confirms the unfair advantage isn't a checklist feature a competitor could bolt on — it's a different paradigm (immediate gate vs. batch queue) plus a different domain (academic/LLM-aware vs. generic HPC).

**Impact:**
Downstream architecture work should not compromise the "immediate gate" model to gain generic-scheduler compatibility — that would trade away the actual differentiator.

**Alternatives considered:**
- N/A — directly stress-tested and confirmed, not a choice between options

**Documented in:** `01-product-brief.md` → Competitive Landscape

---

## Decision 6: Platform = responsive web, desktop-first, live-state only

**Date:** 2026-08-31
**Step:** 10a — Platform Strategy
**Session:** 1

**Context:**
Needed primary platform, device priority, offline needs, native feature needs before UX/design work can start.

**What was decided:**
Responsive web app (no native/PWA/desktop binary), desktop-first with mobile as secondary view-only, no offline mode, no native device features — plain web APIs + REST/WebSocket.

**Why:**
All primary/secondary users work from laptops or lab workstations; data-dense governance views (fleet matrices, reports) need desktop screen real estate. Live infrastructure state makes offline mode actively counter-productive, not just unnecessary.

**Impact:**
UX design (Phase 4) should default to desktop layouts and treat mobile as read-only companion, not full parity. Architecture needs real-time/near-real-time data delivery (e.g. WebSocket) for live fleet status, but no offline-sync/service-worker complexity.

**Alternatives considered:**
- PWA with offline caching — rejected, offline state would be actively misleading for a live-capacity system
- Mobile-first — rejected, doesn't match where users actually work or the data-density needs of governance views

**Documented in:** `01-product-brief.md` → Platform & Device Strategy

---

## Decision 7: Tone of voice = precise, calm, direct, quietly authoritative

**Date:** 2026-08-31
**Step:** 11 — Tone of Voice
**Session:** 1

**Context:**
Agent-proposed (not user-authored) tone attributes for UI microcopy, based on the full product context gathered so far — deterministic denial contract, stressed/time-pressured primary users, trust-through-explainability positioning.

**What was decided:**
Four attributes: Precise & Explicit, Calm & Non-punitive, Direct & Efficient, Quietly Authoritative. Denial messages must always name the specific rule/cause with concrete detail, never generic error codes.

**Why:**
The product's entire differentiator is explainable denials — microcopy that hides or softens the cause would undermine the core value proposition. Users are frequently mid-task under time pressure, so brevity and calm (non-blaming) framing matter more than warmth.

**Impact:**
Any future UI copy (error states, success states, empty states) should be checked against these four attributes and the Do/Don't guidelines before shipping.

**Alternatives considered:**
- Warmer/friendlier consumer-app tone — rejected, would undercut the "quietly authoritative, auditable" positioning this product needs with Administration

**Documented in:** `01-product-brief.md` → Tone of Voice

---

### Product Brief Synthesis (Step 12)

**Final narrative presented:** Confirmed, no adjustments

**Adjustments during synthesis:**
- None

**User confirmation:** Confirmed

**Brief generated:** `../01-product-brief.md`

**Completion:** 2026-08-31

---

_Continue appending decisions as they're made throughout the Product Brief process._
