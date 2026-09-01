---
id: SPEC-gpu-governance
companions: [stakeholders.md, tone-of-voice.md, ARCHITECTURE-SPINE.md, companion-files/glossary.md, companion-files/persona-archetypes.md]
sources: ["../../../design-artifacts/A-Product-Brief/project-brief.md", "../../../../Spec1-Juan_Camilo_Molina_Mussen-A00399775.pdf", "../../../../project - university_ai_compute_management_platform_brief.pdf"]
---

> **Canonical contract.** This SPEC and the files in `companions:` are the complete, preservation-validated contract for what to build, test, and validate. Source documents listed in frontmatter are for traceability only — consult them only if you need narrative rationale or prose color this contract intentionally omits.

# University AI Compute Management Platform

## Why

The university's local GPU fleet (mostly 24GB machines, plus one dual-48GB machine) is coordinated informally via Slack and spreadsheets, and that coordination is breaking down: teachers can't trust that machines needed for scheduled classes will be free, students get opaque rejections with no explanation, lab staff act as manual traffic cops, and academic administration has no way to enforce or demonstrate fair, policy-aligned usage. This is a governance/trust problem, not a convenience-scheduling one — the fix is a deterministic, auditable evaluation gate, not a better queue. Nothing else in the alternative landscape (Slurm/PBS, raw Kubernetes/Kubeflow, buying more GPUs) closes this gap: it's a paradigm difference (immediate zero-queue evaluation vs. long-running HPC batch scheduling) combined with a domain difference (academic-context-aware rules plus LLM token governance), not a feature Slurm could bolt on. The work carries a deadline — specified and architected by the academic evaluation cycle — but the driving force is the operational breakdown itself.

## Capabilities

- **CAP-1**
  - **intent:** System associates each request (access/usage, job-execution, reservation) with the requester's academic context (course, group, privilege level) before evaluating it.
  - **success:** Every evaluated request's decision record shows a resolved academic-context value at the point of evaluation; none are evaluated with an unresolved context.

- **CAP-2**
  - **intent:** System denies an access/usage, job-execution, or reservation request when the requester's academic context is not authorized for the requested model, service, workload type, or resource tier.
  - **success:** An unauthorized request is denied `Denied: Not Authorized` regardless of resource availability; an authorized request is never denied for authorization reasons.

- **CAP-3**
  - **intent:** System denies a job-execution request that violates the time-window and/or authorized-user restriction defined for its workload type.
  - **success:** A request outside its workload type's allowed window or user set is denied. (How multiple simultaneously-applicable restrictions combine is an open question — see Open Questions.)

- **CAP-4**
  - **intent:** System denies any request when no resource currently satisfies it at evaluation time, with no holding, queuing, or rescheduling.
  - **success:** A `Denied: Resource Unavailable` decision is terminal within that same evaluation — never held, queued, or auto-retried.

- **CAP-5**
  - **intent:** System creates a reservation for GPU/VRAM resources tied to an academic activity over a defined time window.
  - **success:** Once created, the resource shows reserved for that activity during its window, and conflicting general-use requests submitted in that window are denied (CAP-6).

- **CAP-6**
  - **intent:** System detects a new request that conflicts with an existing reservation or operational restriction and rejects it.
  - **success:** A colliding request is denied `Denied: Reservation Conflict`, and the response names the specific conflicting reservation/restriction.

- **CAP-7**
  - **intent:** System reports, for any denied request, the specific attributable cause (authorization rule, quota, resource unavailability, or reservation/restriction conflict).
  - **success:** The requester can identify the exact cause directly from the response — never a generic rejection.

- **CAP-8**
  - **intent:** System reports the current status of any tracked machine as available, reserved, busy, idle, or failing.
  - **success:** A lab operator can retrieve a tracked machine's current status on demand.

- **CAP-9**
  - **intent:** System records token-level usage (prompt/generation tokens) and job-level usage per user/course for every accepted request.
  - **success:** Every accepted request produces a queryable usage record — requester, resource, time, and token- or job-level consumption.

- **CAP-10**
  - **intent:** System produces a usage summary, per user or academic context, of resource consumption over a specified period.
  - **success:** A usage summary for a given user/context and time period can be retrieved, reflecting the usage records generated during it.

- **CAP-11**
  - **intent:** System restricts high-VRAM, exclusive, or distributed-training requests to users/contexts explicitly marked as authorized for them.
  - **success:** Such a request is denied for authorization reasons when the requester isn't marked authorized, and is not denied for authorization reasons when marked authorized — independent of a separate unavailability/conflict denial.

- **CAP-12**
  - **intent:** System communicates, on acceptance, which resource was allocated and its conditions of use (time window, expiry).
  - **success:** An Accepted response names the specific allocated resource and its usage conditions directly in the response.

- **CAP-13**
  - **intent:** System states a known future availability window in a `Denied: Resource Unavailable` response only when an existing reservation already establishes when the resource becomes free; it never infers, estimates, or promises a window from any other source.
  - **success:** A `Denied: Resource Unavailable` response for a resource with an existing future reservation includes that reservation's start time as the stated window; the same denial for a resource with no existing future reservation states no window.

## Constraints

- Heterogeneous GPU fleet: mostly 24GB VRAM machines, one dual-48GB machine — acceptance decisions must account for this variance.
- Permission and quota rules are tied to academic context (course/group/privilege level), not identity alone.
- Not all models/services are available to all subjects or courses.
- Workload types (e.g. training jobs) must be restrictable by time window and/or authorized-user set; how the two combine per workload type is unresolved (see Open Questions).
- Reservations must guarantee resource availability for their scheduled academic window — implemented as full precedence over general use, a risk-ranked assumption, not an explicit source requirement (see Assumptions).
- Usage must be tracked and reportable in governance terms (token consumption, infrastructure utilization) — never for billing or chargeback.
- Machine-level operational status must be visible at minimum to lab operators; broader role-based visibility is unresolved (see Open Questions).
- The platform must integrate with the university's existing Kubernetes-managed GPU fleet — GPUs exposed via device plugins, distributed training orchestrated through the Kubeflow Training Operator — rather than select, replace, or bypass that orchestration layer; any acceptance/scheduling logic this spec requires must be satisfiable against that exposure model.
- No latency/throughput/SLA figures exist in source beyond sub-second deterministic evaluation; none are invented here.
- Real-time/near-real-time delivery (e.g. WebSocket) is required for live fleet status; no offline mode, offline-sync, or service-worker caching architecture — state must always reflect current reality.
- Desktop-first responsive web app; mobile is a lightweight read-only companion view, not a full-parity surface.

## Non-goals

- Final internal module decomposition of the platform.
- Exact database schema.
- Exact user interface design, frontend styling, or university branding guidelines.
- Final deployment architecture.
- Exact technology stack beyond the established Kubernetes/Kubeflow context.
- Billing, invoicing, or financial chargeback mechanisms of any kind.
- Request queuing or waitlisting when no resource is available, for this iteration (whether to add it later is an open question, not assumed either way).
- Native mobile apps, desktop binaries, PWA, or offline functionality.

## Success signal

Zero class interruptions per semester due to GPU resource conflicts — the primary metric, overriding all others. Every request resolves deterministically to exactly one of the four outcomes (`Accepted` / `Denied: Not Authorized` / `Denied: Resource Unavailable` / `Denied: Reservation Conflict`) for a given system state, and an identical request under unchanged state always yields the identical outcome.

## Assumptions

- User identity, course/group membership, and privilege level are already available via an existing institutional directory; this platform is not responsible for creating that identity data. [SAFE]
- "Academic context" is one attribute set per request (course + group + privilege level), not a more complex structure like multiple concurrent enrollments with different permissions. [RISKY]
- Reservations take full precedence over general usage during their window, with no partial sharing or negotiation. [RISKY]
- The platform tracks usage for governance/reporting only — never billing or financial chargeback. [SAFE]
- An already-running job is not preempted by a later-arriving, higher-priority reservation; preemption would be an additional capability if required. [RISKY]

## Open Questions

- Should the platform support queuing/waitlisting a request when no resource is available? Explicitly unresolved by source; out of scope this iteration either way.
- For a given workload type, do time-window and authorized-user restrictions both have to hold (AND), does either suffice (OR), or are they applied independently per workload type?
- Should machine operational status be visible to roles beyond lab operators (e.g. students, teachers), and if so at what detail level?
