# Changelog

All notable changes to this project will be documented in this file.

## Unreleased

### Changed

- Detect material implicit choices at handoff, during implementation, and in review before checking authority and derivation.
- Trace indirect behavior, side-effect lifecycle, and environment-to-policy assumptions while preserving accepted choices and limiting blocking to dependent work.
- Align activation and user-facing metadata; add artifact-backed behavioral cases and distinguish static validation from agent execution.

## 0.1.0 - 2026-08-25

### Added

- Initial public release of the `decision-preflight` skill.
- Claim-specific evidence classification and decision-boundary workflow.
- Four decision-state verdicts with fail-closed recommendation behavior.
- Positive, negative, verdict, and ownership activation cases.
- Skills-only Codex plugin manifest, CI validation, security policy, and public documentation.
