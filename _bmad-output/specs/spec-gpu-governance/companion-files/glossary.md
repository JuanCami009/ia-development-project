---
companion-of: SPEC.md, ARCHITECTURE-SPINE.md
---

# Glossary

Canonical domain terms for the University AI Compute Management Platform. Every term below is used with this exact meaning in `SPEC.md`, `ARCHITECTURE-SPINE.md`, and both diagrams — no synonyms, no silent redefinition.

**Academic Context** — The fixed attribute set attached to every request: `user_id`, `course_id`, `group_id`, `privilege_level`. Resolved once, before evaluation (AD-5), never re-derived mid-decision. One attribute set per request — not multiple concurrent enrollments (a flagged `[RISKY]` assumption).

**Privilege Level** — One of five values, closed enum: `student`, `teacher`, `lab_operator`, `admin`, `privileged_researcher` (AD-5). Governs authorization-class checks (CAP-2, CAP-11) and read-boundary rules (AD-11).

**Request** — An incoming ask the platform must evaluate, one of three kinds: **access/usage** (use a model/service), **job-execution** (run a workload, incl. training jobs), or **reservation** (claim a resource for a future academic activity).

**Outcome** — The closed, four-way result of evaluating a request (AD-1): `Accepted`, `Denied: Not Authorized`, `Denied: Resource Unavailable`, `Denied: Reservation Conflict`. Nothing else — no "held," "pending," or "queued" state exists in this outcome type.

**Reason Code** — The machine-readable cause on a `Denied` outcome: `NOT_AUTHORIZED`, `RESOURCE_UNAVAILABLE`, `RESERVATION_CONFLICT` (AD-6). Every denial carries exactly one — CAP-7's "specific attributable cause."

**Known Availability Window** — On a `Denied: Resource Unavailable` response, the start time of the resource's next existing reservation, included only when that reservation is already present in the evaluation snapshot (CAP-13, AD-14). Never inferred, estimated, or looked up live — `null` when no such reservation exists.

**Evaluation Snapshot** — The single, already-resolved bundle of academic context + resource state + reservation data passed into the pure `evaluate()` function (AD-3, AD-5, AD-9). The core reads only this; it makes no live lookups of its own.

**Reservation** — A claim on GPU/VRAM resources tied to a specific academic activity over a defined time window (CAP-5). Takes full precedence over general-use requests during that window — no partial sharing, no negotiation (a flagged `[RISKY]` assumption).

**Reservation Conflict** — What a new request triggers when it collides with an existing reservation or operational restriction (CAP-6); denied with the specific conflicting reservation named in the response, never a generic rejection.

**Machine / Resource** — A single tracked GPU-bearing node in the fleet. Tracked status: `available`, `reserved`, `busy`, `idle`, `failing` (CAP-8). The fleet is heterogeneous — mostly 24GB-VRAM standard nodes, plus one dual-48GB high-capacity node — and acceptance decisions must account for that variance.

**Allocation Ledger** — The PostgreSQL table set that is the sole source of truth for resource/reservation state (AD-4). Kubernetes is never read live as a substitute at decision time; K8s-observed reality enters the ledger only through the health-sync write path.

**Usage Record** — The queryable record produced for every accepted request (CAP-9), attributing token-level (prompt/generation) or job-level consumption to a requester, resource, and time — written in the same transaction as the acceptance (AD-7). Governance/reporting only, never billing.

**Device Plugin** — The Kubernetes mechanism by which the platform's GPU fleet exposes its GPUs to the cluster scheduler. The platform integrates with this exposure model rather than replacing or bypassing it (Constraints).

**Kubeflow Training Operator** — The CRD-based mechanism (TFJob/PyTorchJob under `kubeflow.org/v1`, or TrainJob under the newer `trainer.kubeflow.org` v2 API — exact version deferred, see ARCHITECTURE-SPINE.md Deferred) used for multi-node distributed training jobs.

**Health-Sync** — The one-way write path by which the `k8s` adapter reports observed cluster reality (a node going `failing`, a pod dying outside the platform's control) into the Postgres allocation ledger — never a live read substituted for it at decision time (AD-4, AD-8).

**Determinism Guarantee** — The platform's primary success-signal property: an identical request under unchanged system state always yields the identical `Outcome`. Enforced structurally by the core's purity (AD-9), not by convention.

**Least-Privilege RBAC** — The platform's own Kubernetes `ServiceAccount` is scoped to exactly the resources the `k8s` adapter touches — never cluster-admin or namespace-admin (AD-12).

## Deliberately absent terms

Two terms a reader familiar with HPC/batch schedulers might expect are **not** in this glossary, on purpose:

- **Preemption** — not a platform concept. An already-running job is never displaced by a later, higher-priority reservation this iteration (a flagged `[RISKY]` assumption in `SPEC.md`); adding it would require a new `Outcome` variant and reopen AD-1.
- **Queuing / Waitlisting** — not a platform concept. Every request resolves synchronously, in a single pass, to a terminal `Outcome` (AD-2); holding a request for later re-evaluation is explicitly ruled out (CAP-4, Non-Goals).
