# Decision Preflight

> **Evidence before architecture.**

**Decision Preflight is an evidence gate for AI coding agents.** It detects material implicit choices, then separates current behavior, available capability, accepted intent, and derived requirements before a technical decision becomes architecture or implementation behavior.

[![skills.sh](https://skills.sh/b/KashapovK/decision-preflight)](https://skills.sh/KashapovK/decision-preflight) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## The problem

Technical decisions often inherit assumptions from artifacts that do not own product intent:

- current code proves what exists, not what should exist;
- a backend or library capability does not create a requirement;
- a mock or fixture proves one scenario, not a general contract;
- an accepted decision may need reopening when verified evidence contradicts it;
- speculative future value is not a present requirement;
- a fully specified task or accepted design may leave material behavior undecided, including behavior inherited through defaults, omitted arguments, helpers, or environment assumptions.

Decision Preflight first detects those choices at handoff, during implementation, or in review, then classifies their grounds before dependent work proceeds. Materiality requires a concrete effect on contracts, security, observable behavior, state, recovery, concurrency, or execution conditions.

## Quick start

Install every skill in the repository:

```bash
npx skills add KashapovK/decision-preflight
```

Install only `decision-preflight`:

```bash
npx skills add KashapovK/decision-preflight --skill decision-preflight
```

Try it without installing:

```bash
npx skills use KashapovK/decision-preflight@decision-preflight
```

Update a project installation:

```bash
npx skills update decision-preflight -p -y
```

## What it returns

The skill keeps its default result compact:

- the decision boundary;
- established constraints with provenance;
- rejected assumptions;
- material open gaps;
- a supported recommendation when one is available;
- one decision-state verdict.

| Verdict | Meaning |
|---|---|
| `READY_FOR_DECISION` | Material facts are established; a genuine owner trade-off remains. |
| `READY_TO_PUBLISH` | The selected recommendation is supported and has no material hidden gap. |
| `INCONCLUSIVE` | A factual gap, contradiction, missing authority, or prerequisite may change the outcome. |
| `REOPEN_REQUIRED` | Verified evidence conflicts with a settled decision and owner action is required. |

## How it works

1. Detect material choices, including indirect behavior, before checking their grounds.
2. Set the decision boundary and preserve accepted choices and supported derivations.
3. Establish only the claims that could change the outcome, researching factual gaps.
4. Separate independent decision axes and reject candidates that violate supported hard invariants.
5. Prefer the smallest correct baseline over unproven future-proofing.

For side effects, inspect execution timing and context, state lifetime and sharing, cancellation, reruns, and partial failure. An environment property must have a supported link to intended policy. Review checks both missing required behavior and material behavior that requirements never addressed; the latter needs a concrete consequence and missing basis, not a quotation of a nonexistent requirement. Only work dependent on an unresolved material choice is blocked.

The skill asks the user only for unrecorded intent, a genuine trade-off, or permission to revisit a settled decision. It does not mutate issue trackers or decision records without authorization.

## When it stays out

The skill skips renaming, formatting, equivalent computations, routine debugging, and implementation or ordinary review that only follows settled behavior. An unusual accepted value is not itself a reason to reopen a decision.

A review that uncovers a new material choice is in scope, even with an accepted design. Preserve what was accepted and evaluate only the uncovered behavior. Reopening an accepted choice requires verified conflicting evidence and the applicable owner action.

## Activation and other workflows

Implicit invocation is enabled in `agents/openai.yaml`. Codex can select the skill when the task matches its `SKILL.md` description, including material choices exposed during handoff, implementation, or review. Selection is model-driven, not a guaranteed hook on every code change. Explicitly invoke `$decision-preflight` when you want to request it directly. See [Codex skill activation](https://learn.chatgpt.com/docs/build-skills#how-chatgpt-and-codex-use-skills).

This package defines no hooks or automatic chain of skill calls. Another workflow's instructions can direct the agent to use `decision-preflight` when it is installed and relevant; that workflow keeps ownership of implementation, review, and publication. No companion skill is required. For a project-wide routing instruction, use the rule below.

## Project instructions

A consuming project can add this short routing rule to `AGENTS.md` without copying the procedure:

> Use `decision-preflight` at implementation handoff, during implementation, and in review when the work exposes a material choice not covered by accepted decisions or supported derivations, even if the task is labeled fully specified. Preserve covered decisions, skip mechanical changes, and pause only work dependent on an unresolved material choice.

## Privacy and security

Keep credentials, private source code, personal data, and proprietary endpoints out of external documentation or search services. Use the minimum evidence needed for the decision and preserve the owning repository's authorization boundaries. See the [security policy](SECURITY.md).

## Development

Validate the skill with the repository's pinned Agent Skills reference validator:

```bash
skills-ref validate ./skills/decision-preflight
```

The CI workflow also validates the plugin manifest, checks release-version consistency and whitespace, and scans for secrets. These are static checks, not evidence of agent behavior. See the [behavioral cases and execution protocol](tests/activation-cases.md) and [implicit-choice cases](tests/implicit-decisions.md) for behavioral coverage and execution status.

## Project information

- [Agent Skills specification](https://agentskills.io/specification)
- [MIT license](LICENSE)
- [Security policy](SECURITY.md)
- [Changelog](CHANGELOG.md)
