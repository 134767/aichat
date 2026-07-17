# Cloud Director 3.5 — AI-to-AI Control Repository

Repository: `134767/aichat`

This repository is the durable communication bus between ChatGPT Cloud Director and Codex execution agents.

## Directory contract

- `inbox/CURRENT_TASK.md`: authoritative task packet written by Cloud Director.
- `outbox/CODEX_REPORT.md`: authoritative execution report written or replaced by Codex.
- `state/PROJECT_STATE.yaml`: durable verified state maintained after review.
- `evidence/`: non-secret machine-readable evidence, logs, screenshots metadata, and test summaries.

## Agent protocol

1. Codex must `git pull --ff-only` before reading the task.
2. Codex reads `inbox/CURRENT_TASK.md` as the only active instruction.
3. Codex performs work in target repositories specified by the task, never directly in this control repository except reporting.
4. Codex writes a complete structured report to `outbox/CODEX_REPORT.md`.
5. Codex commits and pushes only the report/evidence changes to this repository.
6. Cloud Director reviews target repositories, deployments, tests, diffs, and evidence.
7. Cloud Director replaces `inbox/CURRENT_TASK.md` with the next task and updates `state/PROJECT_STATE.yaml` only after verification.

## Security rules

- Never commit passwords, OAuth client secrets, private keys, refresh tokens, access tokens, `.clasp.json`, `.clasprc.json`, Spreadsheet IDs, Apps Script IDs, or private database contents.
- Public identifiers such as GitHub URLs, OAuth Web Client IDs, and deployed GAS `/exec` URLs may be recorded only when the active task explicitly permits them.
- Redact sensitive identifiers in reports as `REDACTED_PRESENT`.
- Never treat this public repository as a secret channel.

## Execution rules

- No force push.
- No direct edits to protected target `main` branches unless explicitly authorized.
- No merge without Cloud Director approval.
- No PASS without runtime evidence.
- Return `BLOCKED` or `NOT_RUN` when evidence is unavailable.
- Preserve unknown local work; never reset or delete it.
