# Stakeholders

Five groups, two primary (daily direct interaction), three secondary.

| Group | Status | Need | Frustration today | Coping mechanism today |
|---|---|---|---|---|
| **Students** | Primary | Smooth submission, immediate deterministic feedback, exact denial causes | Opaque blocks with no explanation, unpredictable availability | Slack/WhatsApp to lab staff, off-peak timing, blind competition |
| **Teachers** | Primary | Reserved, non-preemptible GPU capacity for defined class time windows | Uncertainty over whether machines will be free during class hours | Manually asking lab staff to kick off other users, broadcasting usage-avoidance requests |
| **Lab Operators / Infrastructure Staff** | Secondary | Real-time visibility into machine status (available/reserved/busy/idle/failing) and workload distribution, plus automated policy enforcement | Acting as human traffic cops with no tooling | Manual SSH checks, ad-hoc scripts |
| **Academic Administration** | Secondary (positioning decision-driver) | Policy governance and traceable usage reporting (tokens/utilization) with fair-share allocation — never financial chargebacks | No way to enforce or demonstrate fair, policy-aligned use | None — informal, unenforceable |
| **Privileged Users / Researchers** | Secondary | Controlled access to high-VRAM resources (the dual-48GB machine) and multi-node distributed training, under explicit authorization | No defined path to high-demand workloads | Ad hoc arrangement with lab staff |

## Behavioral shift (what changes when the platform works)

- Students stop Slack/WhatsApp-ing lab staff for GPU access — they submit through the system and get instant deterministic feedback.
- Teachers stop broadcasting manual pleas or asking lab staff to kick off student jobs — they create reservations and trust capacity is automatically protected.
- Lab staff stop manual SSH "traffic cop" checks — they monitor a real-time fleet health dashboard.

## Trust signal (experience quality)

Students accept `Denied: Resource Unavailable` / `Denied: Reservation Conflict` as fair because the specific conflicting rule or window is named explicitly. Teachers fully rely on the platform for GPU-dependent labs without needing a manual backup plan.
