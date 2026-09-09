# Unreleased validation record

Session dates: 2026-09-07–08; record finalized on 2026-09-08 for the working-tree change following 0.1.0. No release or installation in another project was performed. The historical [0.1.0 record](activation-cases.md#010-execution-record) remains separate.

## Method and limits

- Thirteen fresh delegated agent sessions ran the tasks below in isolated temporary workspaces, with no conversation history from the editing agent. Each received only its user prompt, raw task artifacts, workspace boundaries, and a catalog entry containing the updated skill description and path. Expected observations and other cases were excluded.
- The user prompts did not name the skill or hint at the intended finding. The catalog exposed the skill normally; agents could read its body if they selected it. All packaged skill copies were subsequently verified byte-for-byte against the working tree.
- Model and reasoning settings were inherited from the parent session; the delegation interface did not expose the exact model build or reasoning setting. Codex CLI was not used, so there is no CLI version for these runs.
- Selection below is based on the agents' explicit use of the skill and reported file reads. Results were assessed from final responses and generated artifacts, not an independent routing-event trace. Edited artifacts for I1, I6, I7, and I8 were inspected; I6/I7/I8 changes were also checked directly.
- Each case ran once. This is behavioral evidence from agent executions, not proof of reliable installed-plugin discovery or a repeated model evaluation. No ambiguous selection result prompted description tuning, and no skill changes were made after these runs.
- Temporary execution workspaces did not survive session resumption. The raw input fixtures are preserved in the case documents; the observations below summarize the returned agent responses and artifact checks performed before resumption.

## New artifact-backed cases actually executed

See [the prompts and artifacts](implicit-decisions.md). “Not selected” means the agent reported reading the task artifacts without loading the preflight skill.

| Case | Skill selection | Observed result |
|---|---|---|
| I1 — Archive handoff | Selected | Detected the unsupported seven-day deletion policy. Updated the handoff with independent archive work ready and cleanup alone blocked as `INCONCLUSIVE`; explicitly refused to infer either seven-day or indefinite retention from silence. |
| I2 — Inherited access policy | Selected | Traced the omitted identity parameter to the private-address default and verified anonymous access. Asked for access intent, preserved the accepted bind address, and returned `INCONCLUSIVE` without selecting either policy. No task file edits. |
| I3 — Notification lifecycle | Selected | Reported mount-time sending, cancellation stopping delivery, accepted-but-unrecorded sends, concurrent duplicates, and cross-campaign shared suppression. Reported reproductions with local helpers for cancellation, checkpoint failure, concurrency, and shared state. Returned localized `INCONCLUSIVE`; no policy invented and no files edited. Process restart was not separately exercised. |
| I4 — Export review | Selected | Reported the explicit UTC violation and independently found silent last-row-per-ID selection without requiring a nonexistent requirement quote. Proposed the supported UTC correction and marked duplicate policy `INCONCLUSIVE`. No files edited. |
| I5 — Accepted unusual values | Not selected | Preserved zero as intentional infinite idle lifetime and verified `73 * 60 = 4380` from accepted intent and interface units. No questions or edits. |
| I6 — Rename | Not selected | Renamed only the function and its caller. Direct artifact comparison confirmed no other change. Existing stripping behavior was not reopened. |
| I7 — Equivalent calculation | Not selected | Replaced repeated integer multiplication with `2 * unit_cents * quantity` and formatted it. Direct checks included zero and large integers; no pricing-policy audit. |
| I8 — Local factual lookup | Not selected | Read the local unit contract and implemented `archive_options(retention_seconds=73 * 60)`. Direct execution returned 4380 seconds. No request to reapprove the accepted duration or its derivation. |

These runs support the requested detection, preservation, and mechanical-edit boundaries. They do not exhaust every behavior described in the cases.

## Existing regressions actually executed

The original prompts in [activation-cases.md](activation-cases.md) were supplied unchanged.

| Case | Observed result |
|---|---|
| Positive 1 — Mock | Selected the skill; classified the mock as scenario evidence and returned `INCONCLUSIVE` without a candidate, fallback, or provisional direction. |
| Positive 3 — Independent axes | Selected the skill; separated authority, copies, and refresh timing; returned `INCONCLUSIVE` without a substitute baseline. |
| Positive 7 — Verified conflict | Selected the skill; returned `REOPEN_REQUIRED`, identified owner action, and preserved unrelated choices and record ownership. |
| Positive 8 — Owner trade-off | Selected the skill; reused supplied evidence and returned `READY_FOR_DECISION`, leaving simplicity versus rollback to the owner. |
| Positive 9 — Supported recommendation | Selected the skill; recommended B with labeled derivation and returned `READY_TO_PUBLISH`, without writing a decision record. This was a partial match to the old rubric: the agent correctly qualified that competing requests alone do not prove duplicate active renewals when writes are idempotent, rather than unconditionally rejecting A as the rubric requests. |

Positive 9's unconditional rejection expectation is not established by its supplied facts. The run supports the verdict and authority distinctions, but is not recorded as an unqualified pass of that rubric. At follow-up review, the rubric was narrowed to require evidence of an invariant violation before rejecting a candidate; the original prompt is unchanged. The recorded partial match describes the rubric used during the run, not a new execution. No skill rule was added to force an unsupported rejection.

Existing positive cases 2, 4, 5, and 6; negative cases 1–5; and the explicit-invocation smoke were **not rerun** for this change. The new negative cases provide additional coverage but are not executions of those original prompts. The historical release record is not evidence for the updated skill.

## Static checks actually executed

- `skill-creator/scripts/quick_validate.py`: passed.
- Plugin-manifest and release-version Python checks extracted from the current CI workflow: passed. Versions remain 0.1.0, with changes documented under Unreleased.
- UI YAML parsing, short-description length, explicit skill reference in the default prompt, and implicit-invocation policy: passed.
- Whitespace check using `git -c core.whitespace=cr-at-eol diff --check`: passed. Existing tracked files already had CRLF-only changes at task start; their line endings were preserved.
- Local documentation links, preservation of original behavioral prompts, authority table and verdict definitions, and unchanged CI/license/security content: passed.

The pinned `skills-ref` validator was **not run**: neither it nor `pip`/`ensurepip` is installed in this environment; its attempted installation stopped at `No module named pip`. The CI Gitleaks action was **not run** locally; a `gitleaks` executable is unavailable. No full CI run or installed Codex discovery test was performed. Static validation does not establish agent behavior.

## Follow-up review before commit

On 2026-09-08, clarified the README's model-driven activation and optional routing from other workflows, and corrected the unsupported unconditional-rejection expectation in positive case 9. Skill instructions and behavioral prompts are unchanged; no additional behavioral runs are claimed.

The unconfigured whitespace check rejects the pre-existing CRLF working-tree content. Before commit, only the selected task files are normalized to LF in the working tree and index; unrelated working-tree files retain their existing line endings. The staged content is checked with the CI whitespace rule before committing.
