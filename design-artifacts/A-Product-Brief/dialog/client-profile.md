# Client Profile: University AI Compute Management Platform

**Created:** 2026-08-31
**Updated:** 2026-08-31

---

## Organisation

| Field | Value |
|-------|-------|
| **Type** | Internal team — university software initiative / course design artifact |
| **Size** | Small — student/individual project level |
| **Industry** | Higher education — academic IT infrastructure (local GPU compute) |
| **Tech maturity** | Moderate-to-high infra context already exists (Kubernetes, device plugins, Kubeflow Training Operator) — but this is the first dedicated governance/management platform for local GPU resources; no prior software of this kind |
| **Design maturity** | — (no prior design-process experience mentioned) |

---

## People

### Primary Contact — JC
- **Role:** Sole builder / project lead
- **Decision mandate:** Full authority to decide, for this design assignment
- **Notes:** Also the technical contact

### Champion (if different)
- **Name:** — (same as primary contact — no separate internal champion)
- **Role:** —
- **Notes:** —

### Technical Contact
- **Name:** JC
- **Role:** Sole builder / project lead

### Other Stakeholders
| Name | Role | Influence |
|------|------|-----------|
| Academic Administration | Governance & auditability oversight | Approver (policy/governance) |
| Teachers | Class-priority guarantee | Approver (must be satisfied — scheduled class capacity) |
| Lab Operators | Operational visibility into fleet | Advisor / operational stakeholder |

---

## Decision Culture

- **Decision style:** Fast-individual (JC decides, for the spec/design phase); Academic Administration, Teachers, Lab Operators must be satisfied by the outcome but do not co-decide the design itself
- **Approval chain:** JC → academic evaluation cycle (course/assignment handoff)
- **Timeline culture:** Fast-iterative for this specification process

---

## Internal Driver

- **What triggered this project:** Informal GPU coordination breaking down — heterogeneous fleet (24GB machines + one dual-48GB machine) causes resource conflicts, teacher uncertainty ahead of scheduled classes, and student friction from opaque/unexplained request rejections
- **What success means internally:** Relief from daily manual coordination; clear auditability/reporting to administration; guaranteed infrastructure availability for scheduled academic classes
- **Internal deadline or pressure:** Yes — handoff-ready specification and architecture needed for the academic evaluation cycle

---

## Working Style

- **Communication preference:** — (not yet discussed)
- **Prior agency experience:** No — internal/solo project, not agency-commissioned
- **Notes:** Source materials (SPEC-1, Student Project Brief) are referenced with citation markers in JC's answers, suggesting a formal document exists but has not yet been shared in full — worth pulling in verbatim later if precision on exact requirements language matters
