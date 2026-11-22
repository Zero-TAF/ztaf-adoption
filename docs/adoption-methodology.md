# The Zero-TAF Adoption Methodology

**Version 0.1.0 — 2026-11-22. Methodology-level guidance. Normative clause text lands in `ztaf-spec` (Wave 2); conformance fixtures in `ztaf-conformance` (Wave 5).**

An organization practices Zero-TAF by taking every consequential agent decision class through the same five stages. The stages loop: estate change returns the organization to Inventory; mandate change returns the affected classes to Prove.

> **Zero Tough.** No agent, tool, delegation, or action is implicitly trusted — including the agents you built yourself, on day one of adoption.

---

## The five stages

### Stage 1 — Inventory (survey the decisions)

Enumerate the agentic estate and classify it. An organization cannot bound what it has not enumerated.

- Inventory every agent, harness, gateway, connector, tool, and credential that can act.
- For each, enumerate the **decision classes** it can take — not the tools it can call: "approve a refund up to €500", "merge to production", "email a customer", "delete a database".
- Score each decision class on the **five risk axes**: reversibility, blast radius, data sensitivity, regulatory exposure, confidence in the observed evidence.
- Record the current reality honestly: which decisions currently run on standing credentials, one-time approvals, or nothing at all.

**Output:** the agent-and-decision inventory, with risk-axis scores. Nothing else in the methodology may proceed past an unenumerated agent.

### Stage 2 — Mandate (constitute the authority)

For each decision class, write the mandate: the signed statement of decision rights.

- Define intended outcomes, constraints in force, and escalation thresholds — the boundary beyond which the agent must escalate (the AOP programme's authority-boundary concept, generalized).
- Assign the **minimum Tough Grade** the mandate must hold, driven by the highest-scoring decision class it governs (ADR-ZTAF-0005). An organization that cannot yet evidence TG-2 declares TG-1 or TG-0 — grade inflation is a conformance failure, not a marketing choice.
- Name the human originator for each chain root. Every mandate descends from an accountable principal.

**Output:** the mandate register — decision classes, mandates, target grades, chain roots.

### Stage 3 — Instrument (place the decision point)

Stand up the machinery that makes decisions verifiable.

- Deploy the **decision point outside the model context**: deterministic policy decision and enforcement that the agent cannot influence, evaluate per action, fail closed.
- Wire **task-identity minting**: agents authenticate per task, bound to their mandate — no durable agent principal carries authority between tasks.
- Wire **credential brokering**: credentials issued after authorisation, scoped to one action class, never persisted (zero standing privilege).
- Wire **evidence emission**: every decision emits a record — inputs, mandate hash, rule applied, decision, approver where relevant, outcome — to an append-only store.

**Output:** decision classes running in **shadow mode** — the decision point observes and records, but does not yet block.

### Stage 4 — Prove (evidence and behavioural baseline)

Grade honestly, before you enforce.

- Run shadow mode long enough to characterise behaviour: off-mandate rate, escalation precision, false-block rate.
- Establish the **behavioural baseline** per decision class (Stage-4 inputs to behavioural verification: drift, barrier responses, canaries).
- Run the must-fail cases: actions that must be blocked, escalations that must fire, evidence records that must verify. A decision class that fails its must-fail set stays in shadow mode.
- Verify the evidence independently: a party that does not operate the agent must be able to verify the records.

**Output:** graded decision classes — Tough Grade assigned on the strength of the evidence, not the ambition of the mandate.

### Stage 5 — Operate (enforce, escalate, re-grade)

Move from shadow to enforce, and stay honest.

- Flip enforced classes from observe to block; enforcement fails closed.
- Operate oversight on **decision cases**: approvers receive the evidence bundle — context, matched rule, proposed action, options — and rule on the case, not on a prompt.
- Monitor behavioural verification continuously; canaries and drift alarms are operational, not decorative.
- **Demote on incident:** an incident in a decision class pulls its grade down until re-proven through Stage 4. Grades are earned continuously, not annually.
- Re-enter Stage 1 on estate change; re-enter Stage 4 on mandate change.

**Output:** operating zero-trust agency — every consequential decision verified against a mandate, bounded at runtime, evidenced independently, and graded.

---

## Roles

| Role | Accountable for |
|---|---|
| Mandate author | Writing and maintaining mandates; naming chain roots; setting target grades |
| Decision-point operator | Running the out-of-model decision point; shadow→enforce transitions; fail-closed behaviour |
| Evidence custodian | The append-only store; independent verifiability; retention |
| Approver | Ruling on escalated decision cases, on the evidence bundle |
| Assurer | Must-fail fixtures, grade assignment, incident demotion, independent verification |

One person may hold several roles; the approver of a decision class must not be the operator of the agent taking it.

## Anti-patterns

| Anti-pattern | Why it fails |
|---|---|
| Blanket approvals ("always allow") | Recreates standing privilege at the human layer; approval fatigue does the rest. |
| Policy in the prompt or system prompt | The model can be persuaded out of it; it is inside the trust boundary. |
| A checker LLM as the decision point | Probabilistic, model-mediated, and in-context. The decision point is deterministic and outside. |
| Logs as evidence | Mutable, operator-held telemetry is not evidence. Records must be append-only and independently verifiable. |
| Grade inflation | A TG declared above what the evidence supports is worse than TG-0: it is a false attestation. |
| Maturity theatre | Promotion of agents to autonomy levels without grade support — governance optics over runtime proof. |
| Big-bang enforcement | Enforcing before proving blocks the business or trains users to bypass. Shadow mode first, always. |

## The first 90 days

1. **Days 1–15:** Stage 1 on one domain. Pick the domain with the sharpest regulatory exposure — the inventory is fastest where the decisions are most consequential.
2. **Days 16–40:** Stage 2 for that domain's decision classes; target grades set conservatively (TG-1/TG-2). No mandate without a named chain root.
3. **Days 41–70:** Stage 3 in shadow mode on the highest-scoring class first.
4. **Days 71–90:** Stage 4 must-fail runs; assign grades; report to the accountable executive with the evidence, not a summary.
5. **Day 90+:** enforce the first class. One class, fully operated, beats ten mandated.

## Mapping to the NIST tenets (per stage)

Stage 1 grounds distinct agent identification (tenet 1); Stage 2 exercises mandates and constraints (6) and HITL triggers (5); Stage 3 realises linking, delegation and attenuation (2), zero standing privilege (3), short-lived revocable authorization (4), architectural separation of policy from reasoning (7), and deterministic access decisions (8); Stage 4 exercises continuous monitoring (9) and begins immutable evidence (10); Stage 5 operates all ten as a system. Zero-TAF's contribution beyond the tenets is the binding — the mandate artifacts, the decision records, and the grades — which is what Stages 2, 3 and 4 produce.
