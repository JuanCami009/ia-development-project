---
companion-of: SPEC.md, ARCHITECTURE-SPINE.md
source: stakeholders.md (per-entity need/frustration/coping matrix), SPEC.md Assumptions & Constraints
---

# Persona Archetypes

Deep dive on the five stakeholder groups introduced in `stakeholders.md`. Where that file answers *what each group needs*, this file answers *why they act the way they do, where the platform's own rules cut against them, and exactly what they're permitted to do* — the tension a design or implementation decision has to resolve, not invent.

Permission boundaries below cite `privilege_level` (AD-5, `ARCHITECTURE-SPINE.md`) and the capability IDs that gate each group's access — every claim here is traceable to a SPEC.md or ARCHITECTURE-SPINE.md ID, not asserted fresh.

---

## Student — `privilege_level: student`

**Motivation.** Get a GPU session working *now*, with minimal ceremony, for coursework or experimentation. Time pressure is usually external (an assignment deadline), not self-imposed.

**Friction today.** Opaque blocks with no explanation; unpredictable availability; falls back to Slack/WhatsApp-ing lab staff, timing submissions for off-peak hours, or informally racing other students for machines.

**Permission boundary.** Default `privilege_level`; authorized for standard access/usage and job-execution requests (CAP-1, CAP-2) but explicitly *not* for high-VRAM, exclusive, or distributed-training requests unless separately marked authorized (CAP-11) — that authorization path exists, but a plain student identity doesn't grant it by default.

**Tension.** The platform's core promise to this persona — CAP-7's "never a generic rejection" — is also the thing most likely to feel punitive if worded wrong: a deterministic, rule-cited denial is fairer than a Slack ghosting, but only if `tone-of-voice.md`'s calm/non-punitive register actually lands (AD-13). A well-specified system that *sounds* like a bureaucratic wall would fail this persona even while being spec-compliant.

**Trust signal.** Per `stakeholders.md`: a student accepts `Denied: Resource Unavailable` / `Denied: Reservation Conflict` as fair specifically because the conflicting rule or window is named explicitly (CAP-7) — trust is earned by the *specificity* of the denial, not its absence.

---

## Teacher — `privilege_level: teacher`

**Motivation.** An absolute, non-negotiable guarantee that GPU capacity exists at the moment a scheduled class needs it. This persona is protecting a live classroom, not a personal workflow — the cost of failure is public and immediate (a class that can't run the lab it was promised).

**Friction today.** No way to *guarantee* anything — manually asking lab staff to kick students off machines, or broadcasting usage-avoidance requests and hoping they're honored.

**Permission boundary.** Authorized to create reservations (CAP-5) tied to an academic activity and time window; those reservations take full precedence over general use for that window (a `[RISKY]` assumption in SPEC.md, since it's a risk-ranked implementation choice, not an explicit source requirement) and any colliding request is denied with the specific reservation named (CAP-6).

**Tension.** The "full precedence, no partial sharing or negotiation" rule that makes this persona's guarantee absolute is the same rule a Student experiences as a hard wall with zero flexibility. There is no mechanism in this spec for a Teacher to *release* slack in a reservation window back to general use — the guarantee is binary, by design, and that's a real trade-off this persona benefits from and the Student persona pays for.

**Trust signal.** Per `stakeholders.md`: teachers stop needing a manual backup plan entirely once they trust the platform enforces this — the moment they still feel the need to "also ask lab staff just in case," the platform has failed this persona regardless of what the spec says on paper.

---

## Lab Operator / Infrastructure Staff — `privilege_level: lab_operator`

**Motivation.** Stop being a human traffic cop. Wants real-time, trustworthy visibility into fleet health so problems are seen before a student or teacher has to report them.

**Friction today.** Manual SSH checks and ad-hoc scripts; visibility is reactive, not standing infrastructure.

**Permission boundary.** Authorized to retrieve any tracked machine's current status on demand — `available` / `reserved` / `busy` / `idle` / `failing` (CAP-8) — pushed in real time via the platform's single commit-and-publish mechanism (AD-8), not polled.

**Tension — genuinely unresolved, not smoothed over here.** `stakeholders.md` names this persona's need as visibility into "machine status **and workload distribution**." CAP-8 only covers *per-machine* status — not which workloads are running where, or load spread across nodes. `ARCHITECTURE-SPINE.md`'s own Deferred section flags this exact gap as a SPEC/companion scope mismatch, deliberately not invented away at the architecture layer. This persona's full need is **not yet met by SPEC.md as written** — that's a real open item for the next SPEC revision, not a documentation gap in this file.

**Trust signal.** A real-time dashboard that's ever caught being stale (a machine shown `busy` that's actually `failing`) undoes this persona's trust immediately — for a group whose entire pain point today is *unreliable* information, a platform that's merely "usually accurate" is worse than the manual SSH check it's replacing.

---

## Academic Administration — `privilege_level: admin`

**Motivation.** Demonstrate — not just claim — fair, policy-aligned governance to whatever body they answer to (department leadership, budget committees). Needs the platform to produce evidence, not just enforce rules quietly.

**Friction today.** No way to enforce *or demonstrate* fair use; governance today is informal and unenforceable, which is itself the exposure this persona carries.

**Permission boundary.** Can retrieve usage summaries per user or academic context over a specified period (CAP-10), reflecting the usage records CAP-9 generates for every accepted request — reads that bypass the core per AD-11 since they carry no authorization decision of their own, but a cross-user summary still reuses evaluate()'s authorization checks rather than an ad hoc one.

**Tension.** This persona's entire value proposition is auditability, and the platform's Non-Goals explicitly exclude billing/chargeback mechanisms — so the "evidence" this persona gets is governance-shaped (token/utilization patterns per course or user), never financial. If this persona's actual institutional ask ever becomes "show me cost," that's a scope conversation this SPEC has already, deliberately, ruled out.

**Trust signal.** Per `stakeholders.md`, this persona is the "positioning decision-driver" — secondary in daily interaction, but the one whose confidence in the platform's determinism (CAP-2's "an authorized request is never denied for authorization reasons") is what lets them stand behind the system institutionally.

---

## Privileged User / Researcher — `privilege_level: privileged_researcher`

**Motivation.** Access to the resources everyone else on this list is explicitly excluded from by default — the dual-48GB machine, exclusive allocation, multi-node distributed training — for research or advanced-course work that plain student access can't serve.

**Friction today.** No defined path to these workloads at all; access happens through ad hoc arrangement with lab staff, i.e. the same informal coordination this whole platform exists to replace.

**Permission boundary.** The *only* persona for whom CAP-11 resolves in the affirmative: high-VRAM, exclusive, or distributed-training requests are denied for authorization reasons when the requester isn't marked authorized, and — critically — are *not* denied for authorization reasons when they are, independent of a separate unavailability or conflict denial. Marking someone authorized is itself outside this SPEC's scope (who performs the marking, and by what criteria, is not defined — see Workshop 2 finding B.4, still open).

**Tension.** This persona sits directly on the fleet's scarcest resource (one dual-48GB machine) and is the *only* persona whose requests can trigger a `Denied: Reservation Conflict` against a Teacher's protected class window (CAP-6) — the platform's determinism guarantee means this persona will experience hard denials more often than any other, precisely because they're requesting the resource under the most contention.

**Trust signal.** This persona has the least tolerance for an unexplained denial, since their requests are already the platform's edge case — CAP-7's attributable-cause requirement matters most here: a `Denied: Resource Unavailable` with a known future window (CAP-13) is the difference between this persona replanning immediately and this persona going back to ad hoc lab-staff arrangements out of frustration.
