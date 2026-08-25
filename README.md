# Decision Preflight

> **Evidence before architecture.**

**Decision Preflight is an evidence gate for AI coding agents.** It separates current behavior, available capability, accepted intent, and derived requirements before an unresolved technical decision becomes architecture.

[![skills.sh](https://skills.sh/b/KashapovK/decision-preflight)](https://skills.sh/KashapovK/decision-preflight) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## The problem

Technical decisions often inherit assumptions from artifacts that do not own product intent:

- current code proves what exists, not what should exist;
- a backend or library capability does not create a requirement;
- a mock or fixture proves one scenario, not a general contract;
- an accepted decision may need reopening when verified evidence contradicts it;
- speculative future value is not a present requirement.

Decision Preflight classifies those claims before alternatives are compared or a recommendation is recorded.

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

1. Set the decision boundary and preserve the owning workflow.
2. Establish only the claims that could change the outcome.
3. Separate independent decision axes before comparing alternatives.
4. Reject candidates that violate supported hard invariants.
5. Prefer the smallest correct baseline over unproven future-proofing.

The skill asks the user only for unrecorded intent, a genuine trade-off, or permission to revisit a settled decision. It does not mutate issue trackers or decision records without authorization.

## When it stays out

The skill skips routine implementation, debugging, mechanical refactors, and ordinary review of settled decisions. A settled decision becomes relevant only when new evidence materially challenges it.

## Privacy and security

Keep credentials, private source code, personal data, and proprietary endpoints out of external documentation or search services. Use the minimum evidence needed for the decision and preserve the owning repository's authorization boundaries. See the [security policy](SECURITY.md).

## Development

Validate the skill with the repository's pinned Agent Skills reference validator:

```bash
skills-ref validate ./skills/decision-preflight
```

The CI workflow also validates the plugin manifest, checks release-version consistency and whitespace, and scans for secrets.

## Project information

- [Agent Skills specification](https://agentskills.io/specification)
- [MIT license](LICENSE)
- [Security policy](SECURITY.md)
- [Changelog](CHANGELOG.md)
