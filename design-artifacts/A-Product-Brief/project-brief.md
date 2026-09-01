# Project Brief: University AI Compute Management Platform

> Complete Strategic Foundation

**Created:** 2026-08-31
**Author:** JC
**Brief Type:** Complete

---

## Strategic Summary

Informal Slack/spreadsheet coordination of a heterogeneous local GPU fleet (mostly 24GB machines, plus one dual-48GB machine) is breaking down — causing class interruptions, teacher uncertainty, and opaque student rejections. This platform replaces that with a deterministic, auditable governance layer: an immediate synchronous evaluation gate (not a queue) that resolves every request to exactly one of four outcomes — Accepted, Denied: Not Authorized, Denied: Resource Unavailable, or Denied: Reservation Conflict — bridging academic context (course, group, privilege) directly to the Kubernetes-managed fleet, with zero billing or chargeback complexity.

Students and Teachers are the primary daily users — students need smooth submission with exact denial causes, teachers need non-preemptible reserved capacity for class windows. Lab Operators, Academic Administration, and Privileged Researchers are served secondary users, with Administration doubling as the primary positioning decision-driver for governance buy-in. Success is measured operationally, not financially: zero class interruptions per semester is the single metric that overrides all others, backed by 100% explainable denials and fully deterministic, sub-second evaluation.

The structural differentiator versus alternatives (Slurm/PBS, raw Kubeflow, informal coordination, buying more GPUs) is a paradigm gap, not a feature gap — nothing else combines an immediate academic-context-aware evaluation gate with LLM-specific token governance and zero billing scope. Delivery target: a responsive, desktop-first web app, fully specified and architected by the academic evaluation cycle deadline.

---

## Vision

Turn ad-hoc GPU coordination into a deterministic, auditable governance layer — guaranteeing scheduled academic classes never lose GPU capacity to scarcity, giving students transparent, immediate reasons for every denial, and stretching a small heterogeneous fleet (24GB machines plus one dual-48GB machine) to its real limit, entirely outside any billing or chargeback model.

**Key Insights from Discussion:**
- This is a governance/trust product, not a convenience scheduler — the four-status deterministic contract (Accepted / Denied: Not Authorized / Denied: Resource Unavailable / Denied: Reservation Conflict) replaces ambiguity with an explicit, immediate-eval contract.
- Fairness is two-sided by design: absolute protection for scheduled classes, paired with transparent self-service denial reasons for students.
- Explicitly not a billing/chargeback system — success is measured operationally (zero class interruptions, full operator visibility, auditability), not financially.

---

## Positioning Statement

For Academic Administration and Teachers who need scheduled classes protected from GPU scarcity and fair, auditable governance without manual overhead, this platform is a purpose-built academic GPU governance layer that evaluates every compute request immediately against deterministic rules, returning either Accepted or one of three explicit denial reasons. Unlike informal Slack/spreadsheet coordination, generic HPC schedulers (Slurm/PBS), or raw Kubernetes/Kubeflow without a governance layer, it bridges academic context (course, group, privilege) directly to a heterogeneous local GPU fleet — with zero billing or chargeback complexity.

**Breakdown:**

- **Target Customer:** Academic Administration & Teachers (primary decision drivers); Students, Lab Operators, and Privileged Researchers as served stakeholders
- **Need/Opportunity:** Escape informal coordination that can't enforce class priority or explain denials
- **Category:** Purpose-built academic GPU governance platform
- **Key Benefit:** Immediate, deterministic evaluation with 100% explainable denial causes, zero financial/billing complexity
- **Differentiator:** Purpose-built bridge between academic context (course/group/role) and a heterogeneous K8s GPU fleet — deterministic, explainable, non-billing

---

## Business Model

**Type:** Internal / non-commercial institutional infrastructure (neither B2B nor B2C)

**Rationale:**
There is no buyer/seller relationship and no external paying party. The platform is 100% funded by internal university/department IT budget, owned by Academic Administration and Lab Operations. Quotas and permissions — allocated by academic context (course enrollment, group, privilege level) — function as the allocation "currency" in place of money. Token consumption (prompt/generation) is tracked strictly for usage reporting, fair-share governance, and capacity planning — explicitly never for billing, invoicing, or chargebacks.

**Implications:**
- No pricing, plans, procurement flow, or payment UI of any kind belong in this product.
- "Customers" in the traditional sense don't exist — access decisions are driven by academic-context rules, not commercial ones.
- Success/ROI framing for stakeholders must stay operational (uptime, fairness, auditability) rather than financial (revenue, cost recovery).
- Target Users work (next step) replaces the Business Customer Profile step — there is no B2B buyer to profile.

---

## Ideal Customer Profile (ICP)

**Primary Users (daily direct interaction):**

**Students** — Run inference/training workloads on the shared fleet. Frustrated by opaque blocks with no explanation and unpredictable availability. Want smooth submission, immediate deterministic feedback, and exact denial causes. Today they cope via Slack/WhatsApp to lab staff, off-peak timing, or blind competition.

**Teachers** — Deliver scheduled classes needing dedicated GPU capacity for a whole group. Frustrated by uncertainty over whether machines will be free during class hours. Want reserved, non-preemptible capacity for defined time windows. Today they manually ask lab staff to kick off other users, or broadcast usage-avoidance requests.

### Secondary Users

**Lab Operators / Infrastructure Staff** — Need real-time visibility into machine status (available/reserved/busy/idle/failing) and workload distribution, plus automated policy enforcement. Today: manual SSH checks, ad-hoc scripts, acting as human traffic cops.

**Academic Administration** — The positioning decision-driver, but day-to-day focus is policy governance and traceable usage reporting (tokens/utilization) with fair-share allocation — never financial chargebacks. Not a daily-execution user.

**Privileged Users / Researchers** — Need controlled access to high-VRAM resources (the dual-48GB machine) and multi-node distributed training under explicit authorization.

---

## Product Concept

**Core Structural Idea:** Immediate synchronous evaluation gate, not a queue. Every request (access/usage, job execution, reservation) is evaluated once, on receipt, against current system state, academic context, active reservations, and capacity — resolving to exactly one of four terminal outcomes: Accepted, Denied: Not Authorized, Denied: Resource Unavailable, Denied: Reservation Conflict. No holding, no queueing, no automatic retries in this iteration.

**Reservations as state, not a separate path:** Reservations are state entries. Once active, they modify what the gate evaluates, automatically producing `Denied: Reservation Conflict` for any colliding general-use request — no manual intervention needed to protect a scheduled class.

**Why this structure:** Queuing in a scarce, heterogeneous GPU environment would formalize the exact opacity and false-hope problem that made informal coordination fail. An immediate gate trades "maybe later" for an instant, explainable answer — 100% explainable denial reasons for students, an auditable decision trail for administration, automatic protection of class windows.

**Features that stem from this concept:**
- Synchronous request evaluation endpoint (no background queue/worker for admission decisions)
- Reservation-as-state-entry model
- Four-outcome response contract surfaced directly to end users
- No retry/backoff logic in this iteration — a denial is final until resubmitted

---

## Success Criteria

**Primary metric:** Zero class interruptions per semester due to GPU resource conflicts. An interrupted class is an immediate trust-breaker with Teachers and Academic Administration — protecting scheduled academic windows with 100% reliability is the platform's ultimate priority, above all other metrics.

**Secondary metrics:**
- 100% of denied requests carry an explicit, attributable reason (Not Authorized / Resource Unavailable / Reservation Conflict)
- Sub-second, instantaneous evaluation latency per request — no queueing
- 100% decision consistency — identical requests under identical system state yield identical outcomes
- Zero manual escalations to lab operators/administration about "unexplained blocks"

**Behavioral shift (what changes when it's working):**
- Students stop Slack/WhatsApp-ing lab staff for GPU access — they submit through the system and get instant deterministic feedback
- Teachers stop broadcasting manual pleas or asking lab staff to kick off student jobs — they create reservations and trust capacity is automatically protected
- Lab staff stop manual SSH "traffic cop" checks — they monitor a real-time fleet health dashboard

**Trust signal (experience quality):** Students accept `Denied: Resource Unavailable` / `Denied: Reservation Conflict` as fair because the specific conflicting rule or window is named explicitly. Teachers fully rely on the platform for GPU-dependent labs without needing a manual backup plan.

**Timeline:** Fully specified, architected, and validated for handoff by the current academic evaluation cycle deadline.

---

## Competitive Landscape

**Alternatives today:**
- Informal Slack/WhatsApp/spreadsheet coordination — status quo; no enforcement, causes class interruptions and opaque student blocks
- Generic HPC schedulers (Slurm/PBS) — too complex, no academic-context integration, no token tracking, no explainable denial causes
- Raw Kubernetes/Kubeflow without a governance layer — exposes infra complexity, no domain business rules
- Buying more GPUs — cost-prohibitive, doesn't fix governance/observability/fair-share

**Do-nothing scenario (status quo, next 12 months):** As AI adoption grows across courses, ad-hoc GPU conflicts escalate. Teachers experience active class interruptions from unannounced student/researcher GPU occupation. Lab staff burn out as manual traffic cops handling informal Slack complaints, producing fragmented, un-auditable workarounds. Academic Administration remains unable to demonstrate fair, policy-aligned usage or audit allocation.

**Our unfair advantage:**
- Purpose-built academic domain model — directly couples academic primitives (course, group, privilege level, class time windows) with an immediate, deterministic evaluation gate (Accept vs. 3 explicit denial reasons)
- Token-level governance without billing — captures LLM prompt/generation metrics purely for fair-share policy enforcement and operational visibility, with zero financial chargeback overhead

**Reality check (if Slurm/PBS added course/group awareness):** Still wouldn't close the gap. Slurm is architected for long-running HPC batch scheduling and node reservations — a fundamentally different paradigm from an immediate, zero-queue, deterministic evaluation gate for interactive LLM workloads. It also lacks native LLM-specific operational metrics (prompt/generation token tracking) and human-explainable denial feedback tailored to student/teacher UX. The gap is structural (paradigm + domain), not a feature checkbox Slurm could bolt on.

---

## Constraints

**Timeline:**
- Fixed deadline: academic evaluation cycle
- Interim checkpoints: self-paced SDD flow milestones (Brief → Trigger Map → SPEC.md → ARCHITECTURE-SPINE.md)

**Technical:**
- Locked: heterogeneous local GPU fleet (mostly 24GB VRAM machines + one dual-48GB machine) managed via Kubernetes with GPU device plugins and Kubeflow Training Operator
- Deferred (open): language, database engine, web framework, deployment architecture — not locked at this specification phase

**Budget:**
- Fixed: $0 financial chargebacks/billing mechanisms — strictly non-commercial governance
- No hard external cloud budget ceiling for dev/test compute — runs on internal university infrastructure

**Brand/UI:**
- Explicit non-goal for this iteration: exact UI design, frontend styling, university branding guidelines

**What's genuinely flexible (decide later):**
- Internal module decomposition and component boundaries
- Exact database schema and data persistence strategy
- Final implementation tech stack beyond the established K8s/Kubeflow context
- Open question (carried from SPEC-1, explicitly out of scope now): whether future iterations should add request queuing/waitlisting

---

## Platform & Device Strategy

**Primary Platform:** Responsive web application — standard modern browsers. No native mobile apps, desktop binaries, or PWA needed.

**Supported Devices:** Desktop/laptop (primary), mobile (secondary, view-only)

**Device Priority:** Desktop-first. Students, Teachers, Lab Operators, and Administrators all interact from personal laptops or lab workstations. Desktop-first optimizes for data-dense views (dashboard monitoring, resource matrices, token reports). Mobile responsiveness covers secondary view-only needs.

**Interaction Models:** Mouse and keyboard (standard web interaction)

**Technical Requirements:**
- **Offline Functionality:** None — explicitly out of scope and counter-productive. Evaluation requires live infrastructure status (available/reserved/busy/idle/failing) and real-time state consistency.
- **Native Features:** None needed — no camera, local filesystem access, or native OS hooks. Standard Web APIs + HTTP/REST/WebSocket are sufficient.

**Platform Rationale:**
A governance/monitoring tool for a live, shared, scarce resource has no coherent offline mode — state must always reflect current reality. Desktop-first matches where the primary users actually work (lab/personal workstations) and where data-dense governance views (fleet matrices, reports) need the most screen real estate.

**Design Implications:** Layouts should assume desktop screen real estate as the default target, with mobile treated as a lightweight read-only companion view rather than a full parity surface.

**Development Implications:** Real-time/near-real-time data delivery (e.g. WebSocket) needed for live fleet status; no offline-sync or service-worker caching architecture required.

---

## Tone of Voice

**For UI Microcopy & System Messages**

### Tone Attributes

1. **Precise & Explicit**: Every message states the exact cause. Never a generic "error occurred" — the product's whole differentiator is naming the rule that fired.
2. **Calm & Non-punitive**: A denial is a fact of system state, not a scolding. No blame language, no exclamation points on failures.
3. **Direct & Efficient**: Users are often mid-task under time pressure (class starting in 10 minutes, assignment due). No pleasantries padding the message.
4. **Quietly Authoritative**: Confident and matter-of-fact, not bureaucratic/legalese, not overly formal.

### Examples

**Error Messages (Denial):**
- ✅ "Denied — Reservation Conflict. This GPU is reserved for [Course Name] from 14:00–16:00."
- ❌ "Error: Request denied"

**Error Messages (Authorization):**
- ✅ "Denied — Not Authorized. Your group doesn't have privilege level for this workload type."
- ❌ "Access denied"

**Button Text:**
- ✅ "Request GPU" / "Reserve Window"
- ❌ "Submit"

**Success Messages:**
- ✅ "Accepted. Running on gpu-node-04."
- ❌ "Operation completed successfully"

**Empty States:**
- ✅ "No active requests right now."
- ❌ "No results found"

### Guidelines

**Do:**
- Name the specific rule/cause behind every denial (Not Authorized / Resource Unavailable / Reservation Conflict) with concrete detail (which course, which window, which quota)
- Keep success/status messages short and factual (what happened, where it's running)
- Write for someone under time pressure — assume they'll skim

**Don't:**
- Use generic error codes or vague failure language without a named cause
- Add blame-toned or apologetic language to denials — it's a rule firing, not a mistake
- Pad messages with pleasantries or exclamation marks

---

## Additional Context

_None captured yet — this section is revisited at Step 33 (Analyze Brief) once Content & Language, Visual Direction, and Platform Requirements documents exist alongside this one._

---

## Business Context

- **Primary Goal:** Eliminate class interruptions and opaque student denials by replacing informal GPU coordination with a deterministic, auditable governance layer.
- **Solution:** An immediate synchronous evaluation gate — every request resolves to exactly one of four outcomes (Accepted / Denied: Not Authorized / Denied: Resource Unavailable / Denied: Reservation Conflict) — with reservations modeled as state entries that automatically protect scheduled class windows.
- **Target Users:** Students and Teachers (primary, daily interaction); Lab Operators, Academic Administration, and Privileged Researchers (secondary).

*Full strategic analysis (business goals, personas, driving forces) is developed in [Phase 2: Trigger Mapping](../B-Trigger-Map/).*

---

## Next Steps

This complete brief provides strategic foundation for all design work:

- [ ] **Phase 2: Trigger Mapping** - Map user psychology to business goals
- [ ] **Phase 3: PRD Platform** - Define technical foundation
- [ ] **Phase 4: UX Design** - Begin sketching and specifications
- [ ] **Phase 5: Design System** - If enabled, build components
- [ ] **Phase 6: PRD Finalization** - Compile for development handoff

---

**Status:** Product Brief Complete
**Next Phase:** Trigger Mapping (Phase 2)
**Last Updated:** 2026-08-31

---

_Generated by Whiteport Design Studio_
