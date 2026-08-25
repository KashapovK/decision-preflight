---
name: decision-preflight
description: Preflight an unresolved technical or architecture decision when current code, mocks, contracts, design artifacts, prior decisions, or dependency behavior may be mistaken for requirements or intent. Use before comparing alternatives, recording a decision, or revising a challenged recommendation, including a settled decision challenged by new verified evidence. Skip routine implementation, debugging, mechanical refactors, and ordinary review of settled decisions.
license: MIT
metadata:
  author: KashapovK
  short-description: Preflight technical decisions against evidence
  version: "0.1.0"
---

# Decision Preflight

Determine whether the available evidence is sufficient to make, recommend, or publish an unresolved technical decision without turning accidental implementation details into architecture.

Use this skill as a standalone preflight. If an issue, decision record, planning system, or another workflow owns publication and lifecycle state, return the result to it without taking over that ownership.

## Preserve these distinctions

- Current state proves what exists; it does not automatically prove what should exist.
- Availability proves capability; it does not automatically create a requirement.
- Authority is claim-specific. A source may own a current fact without owning desired behavior.
- Derived requirements must show their derivation and remain labeled as derived.
- Hypothetical future value is not a current requirement without explicit provenance.
- A material contradiction or missing authority blocks a confident recommendation when it could change the outcome.

## Choose proportional depth

Inspect only the evidence and decision branches that could change the outcome:

- For a new unresolved decision, establish the material claims before evaluating alternatives.
- For a challenged or revised recommendation, recheck the affected claims and branch rather than reopening unrelated reasoning.
- Reuse a current, trustworthy evidence pack instead of rebuilding it.
- Stop using this skill when the task is routine implementation of a settled decision.

Do not widen the user's scope, silently reopen a settled decision, or mutate external tracker or decision-record state without authorization.

## 1. Set the decision boundary

Identify:

```text
Decision:
Must decide here:
Accepted facts consumed from elsewhere:
Outside this decision:
Destination when resolved, if any:
```

When the project has an ownership model, preserve its boundaries. Report useful adjacent findings separately unless they must be resolved for the current decision.

## 2. Establish material claims

Treat authority as local to the question:

| Source | What it can normally establish |
|---|---|
| Latest explicit user or product decision | Current intent within its stated scope |
| Accepted decision record | Accepted intent for the exact question it resolved |
| Approved product or design specification | Intended behavior for its covered states or surfaces |
| Maintained contract or observed service behavior | Current external contract or server semantics |
| Repository code | Current implementation behavior |
| Tests | Test-encoded expectations, which may still be incomplete or stale |
| Mock, fixture, story, demo, or example | The scenario it models, not general intent |
| Version-matched dependency documentation, source, or tests | Capability at the verified version or channel |
| History or discussion | Past motivation and constraints, limited by the record's status and age |

Classify drafts, planned architecture, placeholders, legacy paths, and unfinished work by their accepted status and by the question they answer. Do not promote or dismiss them solely because of their label.

For each claim that could change the outcome, track as much of this ledger as the decision needs:

```text
Claim:
Evidence and source:
What the source establishes for this claim:
Direct or derived:
Required now:
Confidence or gap:
```

Apply these checks:

- Point to direct evidence for explicit requirements and accepted constraints.
- Record a derived requirement as `A + B -> C`; do not attribute `C` directly to the user or product.
- Label recommendations and preferences as choices rather than requirements.
- Treat backend or library availability as capability until a requirement connects it to the decision.
- Verify material version-sensitive claims against the intended version or channel.
- Scan a representative relevant surface before claiming absence, uniqueness, ownership, or a project-wide convention.

For an apparent constraint from current implementation or an artifact, ask whether it survives replacing that implementation, deleting the example, changing a mutable contract, or substituting the dependency. These counterfactuals challenge the constraint's authority, not the fact that the current state exists.

When evidence disagrees, classify the conflict before choosing a side: scope mismatch, current behavior versus desired intent, stale versus current decision, version mismatch, or genuine unresolved contradiction. Research factual gaps first. Ask the user only for unrecorded intent, a genuine trade-off, or permission to revisit a settled decision.

## 3. Evaluate the decision

Separate independent decision axes before listing alternatives. Choices from different axes may compose, but they are not substitutes.

For each axis:

1. State the question and hard invariants with provenance.
2. Remove candidates that violate a hard invariant.
3. Compare only the surviving candidates on relevant trade-offs such as complexity, security, operability, reversibility, compatibility, and local fit.
4. Export only the constraint or decision owned by this axis.

Define the smallest correct baseline before adding caching, abstraction layers, migration machinery, or other optimization. For an optional mechanism, identify the observed problem, expected gain, new costs and failure modes, and adoption trigger. Without a current or explicitly imminent problem, keep it out of the primary recommendation. When the user raised the mechanism, record it as optional future work with the missing adoption trigger rather than as a requirement.

Check representative local patterns before introducing a new interface, mapper, wrapper, service, state layer, or abstraction. Add one only when it hides real translation or complexity, enforces an invariant, or supports actual variation.

If new evidence changes a recommendation, revise the smallest affected branch and report the previous recommendation, new evidence, reason for the change, affected consumers, and unaffected reasoning.

## 4. Return the smallest useful result

Use a compact result by default:

```markdown
### Decision
<decision and boundary>

### Established constraints
- <constraint and provenance>

### Rejected assumptions
- <assumption and why it was rejected>

### Open gaps
- <material gap, or "None">

### Recommendation
<supported recommendation, or "No supported recommendation yet — <blocking gap>">

### Verdict
READY_FOR_DECISION | READY_TO_PUBLISH | INCONCLUSIVE | REOPEN_REQUIRED — <reason>
```

Omit `Recommendation` when neither requested nor supported. When `INCONCLUSIVE` because choice-determining evidence is missing, `Recommendation` contains exactly one sentence: `No supported recommendation yet — <blocking gap>`. Name no candidate and offer no default, baseline, fallback, conditional decision rule, or provisional direction anywhere in the result. Put the next evidence needed in `Open gaps`, then end the response after the verdict. Use the verdicts precisely:

- `READY_FOR_DECISION`: material facts are established and the remaining uncertainty is a genuine choice or trade-off.
- `READY_TO_PUBLISH`: a recommendation has been selected, its requirements and derivation are supported, and no material hidden gap remains.
- `INCONCLUSIVE`: a factual gap, unresolved contradiction, missing authority, or prerequisite could still change the outcome.
- `REOPEN_REQUIRED`: verified evidence conflicts with a settled decision, and permission or owner action is required before proceeding.

Expand the result into a claim ledger or per-axis comparison only when risk, ambiguity, or the user's request justifies the extra detail. When another workflow owns publication or lifecycle state, return the result and let that workflow record or publish it.
