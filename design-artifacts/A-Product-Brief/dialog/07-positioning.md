# Step 7: Positioning

**Completed:** 2026-08-31
**Session:** 1

---

## Positioning Exploration

**Agent asked:**
"Positioning is critical for stakeholder buy-in — who are you building this for, and what makes it different from alternatives?"

**User's initial response:**
Named Academic Administration & Teachers as primary drivers (Teachers need guaranteed non-preemptible class capacity; Administration needs fair, policy-aligned governance without human-in-the-loop overhead), then listed four concrete alternatives and why each fails, then the core differentiator — all in a single detailed pass.

---

## Key Exchanges

### Differentiation

**Agent:** What makes this different from alternatives?
**User:** Purpose-built academic GPU governance bridging academic context (course/group/role) directly with a heterogeneous K8s GPU fleet — immediate, deterministic evaluation with 100% explainable denial causes and zero financial/billing complexity.

**What this revealed:**
The differentiator isn't a single feature but the combination of academic-context-awareness + determinism + explainability + explicit non-billing scope — none of the four alternatives combine all of these.

---

### Market Context

**Agent:** What would they use instead, and why does it fail?
**User:** (1) Informal Slack/WhatsApp/spreadsheet coordination — status quo, no enforcement, causes class interruptions and opaque student blocks. (2) Generic HPC schedulers (Slurm/PBS) — too complex, no academic-context integration, no token tracking, no explicit denial causes. (3) Raw Kubernetes/Kubeflow without a governance layer — exposes infra complexity, no business rules (reservation precedence, denial attribution). (4) Buying more GPUs — cost-prohibitive, doesn't fix governance/observability/fair-share.

**What this revealed:**
Four distinct alternative categories, each failing for a different structural reason (no enforcement / no domain fit / no governance layer / doesn't address root cause) — confirms this isn't competing on features but on being the only option that combines academic-domain rules with real infrastructure governance.

---

### Value Proposition

**Agent:** (implicit in same answer) What's the key benefit?
**User:** Immediate, deterministic evaluation contract with 100% explainable denial causes, zero financial/billing complexity.

**What this revealed:**
The value proposition is trust and clarity (both for the institution auditing it and the student receiving a denial), not speed or cost savings.

---

## Reflection Checkpoint

**Agent's synthesis:**
"Target Customer: Academic Administration & Teachers (primary). Their Need: escape informal coordination that can't enforce priority or explain denials. Category: purpose-built academic GPU governance platform. Key Benefit: immediate deterministic evaluation, 100% explainable denials, zero billing complexity. Alternatives: informal coordination, generic HPC schedulers, raw K8s/Kubeflow, buying more GPUs. Differentiator: purpose-built bridge between academic context and heterogeneous K8s fleet — deterministic, explainable, non-billing."

**User response:**
- [x] Confirmed
- [ ] Corrected

**Corrections (if any):**
None — confirmed as "captures it perfectly" on first pass.

---

## Positioning Statement

For Academic Administration and Teachers who need scheduled classes protected from GPU scarcity and fair, auditable governance without manual overhead, this platform is a purpose-built academic GPU governance layer that evaluates every compute request immediately against deterministic rules, returning either Accepted or one of three explicit denial reasons. Unlike informal Slack/spreadsheet coordination, generic HPC schedulers (Slurm/PBS), or raw Kubernetes/Kubeflow without a governance layer, it bridges academic context (course, group, privilege) directly to a heterogeneous local GPU fleet — with zero billing or chargeback complexity.

**For:** Academic Administration & Teachers (primary); Students, Lab Operators, Privileged Researchers (served)
**Who:** Need scheduled classes protected from scarcity and governance that's fair and auditable without manual overhead
**This product:** A purpose-built academic GPU governance platform
**That:** Evaluates every request immediately against deterministic rules with 100% explainable denials
**Unlike:** Informal coordination, generic HPC schedulers, raw K8s/Kubeflow, or just buying more GPUs
**Our approach:** Bridge academic context directly to the GPU fleet — deterministic, explainable, never a billing system

---

## Supporting Evidence

**Why this position makes sense:**
1. The status quo (informal coordination) is actively failing — named as the direct trigger for this project, not a hypothetical pain point.
2. Generic infrastructure tools (Slurm, raw Kubeflow) exist but none encode academic-context rules (course/group/privilege, reservation precedence) — there's a genuine gap this fills rather than competes into.
3. Explicitly ruling out billing/chargeback scope keeps the positioning honest about what this is (governance) vs. what it isn't (a cost-allocation or cloud-billing product) — reduces scope creep risk later.

---

**Documented in:** `wds-project-outline.yaml` → `positioning`
