---
name: decision-preflight
description: Detect material implicit choices at implementation handoff, during implementation, or in review, even when a task is fully specified or its design accepted; then check their evidence and authority. Include behavior chosen through defaults, omitted arguments, helpers, or environment assumptions. Also preflight unresolved technical decisions before comparing, recommending, recording, or revising them, including settled decisions challenged by verified evidence. Skip mechanical edits, routine debugging, and implementation or review that only follows settled behavior without a new material choice.
license: MIT
metadata:
  author: KashapovK
  short-description: Detect implicit decisions and verify their grounds
  version: "0.1.0"
---

# Decision Preflight

Detect material choices before checking whether the available evidence is sufficient to make, implement, recommend, or publish them. A task labeled fully specified or an accepted design does not establish intent for behavior it never addressed.

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
- Preserve accepted choices, including unusual values, and explicitly supported derived requirements within their scope. Silence about another behavior is neither acceptance nor a contradiction.
- Stop when implementation or ordinary review only follows settled behavior and exposes no new material choice. Renaming, formatting, and equivalent computations do not warrant an architecture audit.

Do not widen the user's scope, silently reopen a settled decision, or mutate external tracker or decision-record state without authorization.

## 0. Detect material choices

At handoff, implementation, or review, briefly trace what the proposed work makes happen before deciding whether there is an unresolved decision. Look beyond explicit values to defaults, omitted parameters, helper behavior, inherited configuration, and assumptions about the execution environment. Follow only paths relevant to a concrete consequence; do not inventory every possible design choice.

A choice is material when a plausible alternative changes a contract, security boundary, observable behavior, state, failure recovery, concurrency, or execution conditions. Name the behavior and its concrete consequence. Mere stylistic preference or a behaviorally equivalent computation is not enough.

For side effects, establish when and in which context they run, who owns state, how long it lives and who shares it, and what cancellation, rerun, or partial failure leaves behind. Trace relevant callers and helpers instead of assuming a local-looking operation has local effects. When inferring policy from environment properties, verify both the property and the requirement connecting it to that policy; capability, topology, or an observed runtime condition alone does not supply intent.

Match each material behavior to an accepted decision, explicit requirement, or supported derivation. Carry covered behavior forward unchanged. For uncovered behavior, define the smallest unresolved choice and continue below. Verified conflict with an accepted decision follows the existing reopening path; an additional uncovered choice does not reopen that decision by itself.

In review, check both directions: required behavior missing from the implementation, and material implementation behavior absent from the requirements. For the latter, report the behavior and its location, consequences, and missing basis after inspecting relevant evidence. Do not require a quotation of a nonexistent requirement. An ordinary defect against a settled requirement can stay in the normal review workflow.

Block only work that depends on the unresolved material choice; continue independent work. Research factual gaps yourself before asking for genuinely missing intent or an owner choice. Do not fill a policy gap with a conventional or supposedly safe default.

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
- Record a derived requirement as `A + B -> C`; verify the premises and the link to `C`, and do not attribute `C` directly to the user or product. Preserve a supported derivation; do not ask the user to restate it.
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

For a newly exposed implementation choice, include its behavior, consequence, and missing basis in the compact result or review finding. Apply the verdict to that choice, not to unrelated accepted work. Expand the result into a claim ledger or per-axis comparison only when risk, ambiguity, or the user's request justifies the extra detail. When another workflow owns publication or lifecycle state, return the result and let that workflow record or publish it.
