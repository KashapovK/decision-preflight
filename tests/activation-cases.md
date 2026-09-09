# Decision Preflight behavioral cases

Use these cases to evaluate implicit activation and decision quality. Test the skill as installed, without naming `$decision-preflight` in the prompt unless the case explicitly checks manual invocation.

## Observable contract

A successful run should:

- detect material implicit choices before evidence classification, including in work labeled fully specified or accepted;
- activate only where an unresolved material choice or verified conflict could change the outcome;
- distinguish current state, capability, intent, and derived requirements;
- inspect proportionately, preserve accepted choices and supported derivations, and avoid reopening unrelated reasoning;
- trace indirect behavior and side-effect lifecycle when concretely material;
- block only dependent work and research factual gaps before asking for missing intent;
- return the smallest useful result with the correct verdict;
- preserve external issue, decision-record, and tracker ownership;
- work without any named companion skill.

Do not grade exact headings or wording. Grade the decisions, evidence use, scope, verdict, and side effects.

Artifact-backed handoff, implementation, review, and mechanical-edit cases are in [Implicit decision cases](implicit-decisions.md). They extend the regressions below; their [execution status](execution-status.md) is recorded separately from the historical 0.1.0 runs.

## Positive activation cases

### 1. Mock mistaken for a requirement

```text
Our UI story mock contains a persisted `category` field. Should the production domain model and API require that field, or should the UI derive it? Recommend the architecture.
```

Expected observations:

- activates because an unresolved design may be constrained by a mock;
- treats the mock as evidence of a scenario, not product intent;
- looks for authoritative product, contract, and implementation evidence;
- does not name a safe, default, or provisional option when intent is missing;
- returns `INCONCLUSIVE` if the intended semantics cannot be established.

### 2. Capability mistaken for intent

```text
The backend can store per-user view settings, so I recommended persisting this toggle. The reviewer says it should remain local. Recheck the recommendation.
```

Expected observations:

- activates for a challenged recommendation;
- rechecks the affected branch rather than reopening unrelated decisions;
- does not treat storage capability as a persistence requirement;
- explains and localizes any recommendation change.

### 3. Independent axes mixed as alternatives

```text
For this data flow, should we choose server ownership, a local cache, or background refresh? Compare the options and choose one.
```

Expected observations:

- activates before comparing architecture alternatives;
- separates ownership, caching, and refresh lifecycle into independent axes;
- does not rank mechanisms that answer different questions as substitutes;
- does not name a likely baseline or fallback when missing evidence could change the choice.

### 4. Current contract treated as immutable

```text
The existing service returns one combined status, but the approved product states require two independent statuses. Should the client preserve the current contract shape in the new design?
```

Expected observations:

- activates because current behavior and accepted intent conflict;
- establishes what the contract proves without assuming it owns the desired design;
- surfaces whether a contract change is in scope or belongs to another owner.

### 5. Version-sensitive dependency claim

```text
Our architecture depends on a library feature that may exist only in newer releases. Decide whether we can base the design on it for the version this project uses.
```

Expected observations:

- activates because the dependency capability is material to the decision;
- verifies the exact intended version or channel before relying on the feature;
- returns `INCONCLUSIVE` rather than substituting a current-version assumption.

### 6. Premature future-proofing

```text
Traffic is small today, but it might grow later. Should we add a queue, a cache, and a second state store now to future-proof the feature?
```

Expected observations:

- activates for an unresolved architecture recommendation;
- asks for provenance of any current scale or resilience requirement;
- establishes the smallest correct baseline before optional optimization;
- records mechanisms without an adoption trigger as future work, not requirements.

### 7. Accepted decision contradicted by verified evidence

```text
An accepted decision says the client is the security boundary, but the current security policy says authorization must be enforced by the service. Continue with the accepted design or update the recommendation?
```

Expected observations:

- activates because verified evidence may invalidate a settled decision;
- does not silently override or blindly preserve the decision;
- returns `REOPEN_REQUIRED` and identifies the permission or owner action needed.

### 8. Evidence complete, choice still open

```text
Prepare this migration decision for the owner using the following evidence:

- Accepted product requirement: reads must remain available throughout the migration.
- Wire contract: readers may accept schema v1 and v2 for one release.
- Current implementation: readers and writers use schema v1.
- Option A: deploy v1/v2 readers, then cut writers to v2; it has fewer moving parts, but rollback requires redeploying the writer.
- Option B: deploy v1/v2 readers and a versioned writer flag; it adds a control path, but rollback is a flag change.

Both options satisfy the availability requirement. The owner must choose between simpler migration and easier rollback.
```

Expected observations:

- activates and reuses the supplied evidence pack;
- compares only alternatives on the same decision axis;
- returns `READY_FOR_DECISION`, because the remaining uncertainty is a genuine trade-off.

### 9. Recommendation supported and ready to record

```text
Confirm the recommendation before I record this renewal-ownership decision:

- Accepted requirement: a subscription may have at most one active renewal.
- Current contract: renewal writes are idempotent by subscription ID, and the service owns renewal state.
- Representative local pattern: other lifecycle changes use service command handlers.
- Option A: each client schedules renewals locally; two devices can issue competing renewals.
- Option B: one service command handler schedules renewals and enforces idempotency.
```

Expected observations:

- activates before the recommendation is recorded;
- rejects candidates proven to violate the invariant before comparing softer trade-offs; does not infer duplicate active renewals merely from competing requests when the contract makes writes idempotent;
- verifies that option B does not depend on an unproven assumption or optional optimization;
- returns `READY_TO_PUBLISH` when no material gap remains.

## Negative activation cases

### 1. Routine implementation of a settled decision

```text
Implement the accepted ADR: rename the endpoint and update its callers and tests.
```

Expected observation: does not activate; follows the implementation workflow.

### 2. Debugging

```text
This parser throws on an empty input. Find the cause and explain the failing test.
```

Expected observation: does not activate; diagnoses the defect directly.

### 3. Mechanical refactor

```text
Rename `formatAccount` to `displayAccount` and update every caller without changing behavior.
```

Expected observation: does not activate; performs the scoped refactor.

### 4. Ordinary code review

```text
Review this small patch for correctness and regressions. The design is already accepted.
```

Expected observation: does not activate solely because the review mentions design or correctness. With no patch supplied, requests the patch; this does not establish that an unseen implementation contains no material choice. Compare the artifact-backed review cases in [Implicit decision cases](implicit-decisions.md).

### 5. Local implementation choice

```text
Use the project's existing formatter on this component and fix the reported formatting issue.
```

Expected observation: does not activate; this is not an unresolved technical decision.

## Verdict checks

| Evidence state | Expected verdict |
|---|---|
| Material facts are established; a real owner trade-off remains | `READY_FOR_DECISION` |
| A selected recommendation is supported and has no material hidden gap | `READY_TO_PUBLISH` |
| A factual gap, contradiction, missing authority, or prerequisite may change the outcome | `INCONCLUSIVE` |
| Verified evidence conflicts with a settled decision and reopening needs authorization | `REOPEN_REQUIRED` |

The verdict must describe the current decision state. Do not use `READY_TO_PUBLISH` merely because an investigation is complete, and do not use `INCONCLUSIVE` when only an explicit human preference remains.

## Execution protocol

1. Install the skill in an isolated test profile with implicit invocation enabled.
2. Start each case in a fresh conversation and provide only the prompt plus the minimum artifacts it names.
3. For activation cases, do not mention the skill name. Add one separate smoke run that invokes `$decision-preflight` explicitly.
4. Record whether the skill activated, which evidence it classified, whether it widened scope or caused side effects, and which verdict it returned.
5. Repeat ambiguous routing cases at least twice before changing the description from a single result.
6. Refine the narrowest instruction supported by a repeated failure; do not add universal rules for one unusual example.

Static validation is necessary but does not count as a behavioral pass. Preserve execution records with the model, Codex version, date, observed result, and any resulting skill change before publishing a release.

## 0.1.0 execution record

Executed eight targeted cases with Codex CLI `0.149.0-alpha.4.3` and model `gpt-5.6-sol` on 2026-08-25. Every recorded case ran in a fresh ephemeral read-only session against a temporary workspace containing only the packaged `decision-preflight` skill. The CLI reported reasoning effort `none`. No project repository or private artifacts were present.

Positive cases 1 and 3 exposed recommendations that still named a fallback or conditional choice after returning `INCONCLUSIVE`. The output contract was narrowed and both regressions passed in fresh sessions before this record was finalized. Cases not listed below were not run for this release.

| Case | Expected activation | Observed activation | Result |
|---|---|---|---|
| Positive 1 — Mock mistaken for a requirement | Activate | Activated | Pass after refinement — treated the mock as scenario evidence and ended with `INCONCLUSIVE`, without naming a candidate or default. |
| Positive 2 — Capability mistaken for intent | Activate | Activated | Pass — rejected backend capability as proof of persistence intent and returned `INCONCLUSIVE`. |
| Positive 3 — Independent axes | Activate | Activated | Pass after refinement — separated ownership, caching, and refresh; returned `INCONCLUSIVE` without a fallback. |
| Positive 4 — Current contract versus intent | Activate | Activated | Pass — treated the approved product states as intent, isolated legacy translation at the boundary, and returned `READY_TO_PUBLISH`. |
| Positive 7 — Settled decision contradicted | Activate | Activated | Pass — localized the security-policy conflict and returned `REOPEN_REQUIRED` with the required owner action. |
| Positive 8 — Evidence complete, choice open | Activate | Activated | Pass — compared alternatives on one axis and returned `READY_FOR_DECISION` for the owner trade-off. |
| Negative 4 — Ordinary review | Do not activate | Not activated | Pass — stayed within correctness and regressions, then requested the missing patch without reopening design. |
| Explicit invocation smoke | Activate | Activated | Pass — invoked `$decision-preflight`, preserved decision-record ownership, found an unresolved idempotency-lifetime gap, and returned `INCONCLUSIVE`. |
