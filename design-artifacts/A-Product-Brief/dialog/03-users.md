# Step 3: User Definition

**Completed:** 2026-08-31
**Session:** 1

---

## Opening Question

**Agent asked:**
"Which group interacts with the system most/daily (primary)? Walk me through their daily experience, biggest frustration, core goal, and current workaround. Then a quick pass on the rest as secondary."

**User's initial response:**
Named two primary users (Students, Teachers) with full behavioral depth for each, plus three secondary users (Lab Operators, Academic Administration, Privileged Researchers) with role-level detail.

---

## User Exploration

### Primary User Discovery

**Agent:** Who interacts daily — and what's their current pain?
**User:** Students run inference/training jobs and get opaque blocks with no explanation; Teachers need guaranteed reserved capacity for scheduled classes but can't trust it'll be free.

**Agent:** How do they cope today?
**User:** Students Slack/WhatsApp lab staff, try off-peak hours, or blindly compete for resources. Teachers ask lab staff to manually kick other users off machines, or broadcast "please don't use during class" announcements.

**Agent:** Does Academic Administration belong in primary too, given they're the positioning decision-driver?
**User:** No — Admin's day-to-day is reviewing aggregate usage reports and setting policy, not executing daily compute requests. Positioning decision-driver ≠ daily primary user. Keep as secondary/strategic stakeholder.

---

## Reflection Checkpoint

**Agent's synthesis:**
"Primary = Students (submit workloads, want deterministic feedback + clear denial reasons) and Teachers (need non-preemptible reserved capacity for class windows). Secondary = Lab Operators (fleet visibility), Academic Administration (governance/reporting, not daily execution), Privileged Researchers (controlled high-VRAM/multi-node access)."

**User response:**
- [x] Confirmed
- [ ] Corrected

**Corrections (if any):**
None — user affirmed the split as "100% correct" and added the useful distinction that positioning-decision-driver (Admin) is not the same as daily-interaction-primary-user.

---

## Primary User Definition — Students

**Who they are:**
Students running model inference for assignments or executing training jobs for labs/projects on the shared GPU fleet.

**Their context:**
Competing for scarce heterogeneous GPU capacity (mostly 24GB machines, one dual-48GB) alongside scheduled classes and other students' workloads.

**Their frustrations:**
Requests get blocked or fail without clear, explainable reasons; cannot predict GPU availability in advance.

**What they're trying to achieve:**
Submit workloads smoothly, get immediate deterministic feedback, and understand exact limits or rejection causes when denied.

**How they currently solve this:**
Manual Slack/WhatsApp messages to lab staff, attempting jobs at off-peak hours, or competing blindly for resources.

---

## Primary User Definition — Teachers

**Who they are:**
Teachers preparing and delivering scheduled classes that require dedicated GPU capacity for an entire student group at once.

**Their context:**
Class time is fixed and high-stakes; ad-hoc student or research jobs can silently consume the capacity a class needs.

**Their frustrations:**
Uncertainty whether required GPU machines will actually be free during class hours.

**What they're trying to achieve:**
Reserve specific GPU resources for defined time windows with guaranteed, non-preemptible precedence over general-use requests.

**How they currently solve this:**
Asking lab staff to manually kick off other users, or sending broad announcements asking students to avoid machines during class hours.

---

## Secondary Users

**User 2 — Lab Operators / Infrastructure Staff:** Monitor machine health and handle manual access requests today; biggest frustration is zero operational visibility into machine status (available/reserved/busy/idle/failing) or workload distribution. Goal: real-time visibility + automated policy enforcement. Currently rely on manual SSH checks, ad-hoc scripts, acting as human traffic cops.

**User 3 — Academic Administration:** Focused on policy governance, traceable usage reporting (tokens/utilization), and fair-share allocation — explicitly without financial chargebacks. This is the positioning decision-driver, but day-to-day interaction is reviewing reports and setting rules, not executing requests.

**User 4 — Privileged Users / Researchers:** Focused on accessing high-VRAM resources (the dual-48GB machine) and running multi-node distributed training under controlled authorization.

---

## User Scenarios Captured

**Scenario 1:** A student submits a training job mid-semester and it's denied — under the old workaround they'd have no idea why; under this system they get one of the three explicit denial reasons immediately.
**Scenario 2:** A teacher has a class starting in 10 minutes needing the dual-48GB machine — a student's ad-hoc job must not be able to block that reservation.

---

**Documented in:** `wds-project-outline.yaml` → `users`
