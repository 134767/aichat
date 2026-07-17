# ACTIVE TASK PACKET

```yaml
task_id: CD35-LCS-2.2.0-AICHAT-PAGES-RUNTIME-MATRIX-002
mode: CLOUD_DIRECTOR_3_5
issuer: ChatGPT Cloud Director
executor: Codex
status: READY
priority: P0
previous_report_commit: 0eeae427c724160570b8fc34b7f6799d3be6f6a4
```

## Objective

Deploy the isolated frontend from `134767/test` feature branch through `134767/aichat` GitHub Pages, preserving the current `134767/test` production Pages site. Then execute available browser identity checks, primary-account read/write verification, 10/25/100 write stress tests, Sheet integrity checks, and report through this control repository.

## Starting baseline

```yaml
control_repository: 134767/aichat
control_branch: main
target_repository: 134767/test
target_branch: feature/lcs-2.2.0-isolated-runtime-auth-bridge
target_expected_sha: 58e7dae27c039096969ed43778d72378c530600f
target_pr: 3
target_pr_must_remain_draft: true
target_pr_merge_authorized: false
preview_expected_url: https://134767.github.io/aichat/
primary_account: fjulibrs@gmail.com
allowed_browser_origin: https://134767.github.io
isolated_sheet_title: LCS 2.2.0 Isolated Runtime Test DB
isolated_sheet_tab: timestamp_log
```

Previous verified results:

- GAS endpoint reachable without a signed-in session: PASS.
- Deployment executes as owner and permits anonymous bridge loading: PASS.
- Required runtime properties exist: PASS.
- Allowed origin is exactly `https://134767.github.io`: PASS.
- Isolated Sheet is private and owner-only: PASS.
- Exact nine-column header: PASS.
- Static tests: 4/4 PASS.
- Credential scan: PASS.
- PR #3: Draft, open, mergeable, unmerged.

## Constraints

- Never force push.
- Never modify `134767/test/main` directly.
- Never merge or mark PR #3 ready.
- Never repoint or overwrite `https://134767.github.io/test/`.
- Never commit private Google identifiers, secrets, login/session material, local clasp configuration, or browser-profile data.
- Preserve unknown local work.
- Do not weaken server-side identity or allowlist checks.
- Do not claim PASS without direct evidence.
- Do not retry failed stress operations merely to replace failures with successes.
- Visible business UI remains one write button and one timestamp display.
- Do not edit `inbox/CURRENT_TASK.md` or `state/PROJECT_STATE.yaml`.

## Phase 1 — Synchronize

1. Pull `134767/aichat/main` with fast-forward only.
2. Read this file and `outbox/CODEX_REPORT.md`.
3. Record synchronized aichat HEAD as `control_task_sha`.
4. Pull `134767/test` feature branch with fast-forward only.
5. Preserve and report any unexpected local work; never reset it.
6. Confirm target SHA. If newer than the expected SHA, inspect and continue only when it is a valid fast-forward on the same branch.

Run target checks:

```text
node --check js/app.js
npm test
syntax-check every gas-lcs-2.2.0-isolated/*.gs with Node
git diff --check
scan for unresolved runtime placeholders
scan for committed credentials
```

Expected: Node PASS, 4/4 tests, GAS syntax PASS, diff PASS, zero runtime placeholders, zero actual committed credentials.

## Phase 2 — Deploy preview from aichat

Create `.github/workflows/deploy-lcs-isolated-preview.yml` in `134767/aichat/main`.

Workflow requirements:

- Trigger on manual dispatch and when the workflow file changes on main.
- Grant read-content, Pages-write, and OIDC deployment permissions only.
- Use the `github-pages` environment and one deployment concurrency group.
- Check out `134767/test` at the exact target SHA, initially `58e7dae27c039096969ed43778d72378c530600f`.
- Build a minimal `_site` containing only runtime frontend files required by `index.html`; normally `index.html`, `css/`, `js/`, and `.nojekyll`.
- Do not publish GAS source, tests, control files, evidence, repository metadata, or private configuration.
- Verify the runtime config has the expected public deployment values and no placeholders.
- Configure Pages, upload `_site`, and deploy.
- Record workflow run URL, deployment SHA, and final page URL.

Expected page:

```text
https://134767.github.io/aichat/
```

This page shares the already-authorized origin `https://134767.github.io` while leaving `134767/test` Pages unchanged.

Commit workflow separately:

```text
ci: deploy isolated LCS preview from pinned target SHA
```

## Phase 3 — Preview smoke test

Open the preview and verify:

```yaml
http_status: 200
write_button_count: 1
timestamp_container_count: 1
hidden_bridge_iframe_count: 1
protected_timestamp_before_auth: false
browser_persistent_auth_state: false
console_errors: 0
bridge_ready: true
```

Confirm all frontend assets load correctly from `/aichat/`. If path handling fails, prefer a preview-only workflow adjustment. Change target source only if the defect also affects its intended deployment.

## Phase 4 — Primary authorized runtime

Using the existing browser profile for `fjulibrs@gmail.com`:

1. Load the preview.
2. Complete Google sign-in when requested.
3. Confirm authorization and initial read.
4. Record Sheet row count.
5. Press the write button once.
6. Confirm row delta exactly `+1`.
7. Confirm displayed timestamp equals the persisted row.
8. Capture frontend round-trip and backend timing fields.
9. Confirm no authorization state is persistently stored by the app.

Required:

```yaml
primary_auth: PASS
primary_read: PASS
primary_single_write: PASS
primary_row_delta: 1
display_matches_sheet: PASS
```

## Phase 5 — Incognito before sign-in

Open a fresh incognito window without a Google session.

Required before any sign-in:

```yaml
protected_data_exposed: false
write_enabled: false
sheet_row_delta: 0
google_signin_available: true
persistent_auth_state: false
```

Capture this state before optionally signing in.

## Phase 6 — Additional accounts

Shared account:

- Resolve only from an exact approved address or an already available signed-in profile.
- Never infer an address.
- If unavailable, report `BLOCKED_MISSING_EXACT_SHARED_EMAIL` and continue stress tests.

Unauthorized Google account:

- Use only an already available signed-in account not present in the backend allowlist.
- Never request or expose its credentials.
- If unavailable, report `BLOCKED_NO_EXISTING_UNAUTHORIZED_PROFILE`.
- When available, verify identity succeeds but backend access is denied, no protected data appears, write remains disabled, and row delta is zero.
- Run reload/account-switch verification only when that profile exists.

## Phase 7 — Stress matrix

Do not start until primary single read/write passes.

Use existing harness:

```javascript
window.LCS_TEST_HARNESS.burst(count, intervalMs)
window.LCS_TEST_HARNESS.waitForIdle(timeoutMs)
window.LCS_TEST_HARNESS.snapshot()
```

Run sequentially:

```javascript
LCS_TEST_HARNESS.burst(10, 250)
await LCS_TEST_HARNESS.waitForIdle(180000)

LCS_TEST_HARNESS.burst(25, 0)
await LCS_TEST_HARNESS.waitForIdle(180000)

LCS_TEST_HARNESS.burst(100, 0)
await LCS_TEST_HARNESS.waitForIdle(600000)
```

For each run capture:

- requested, successful, failed;
- Sheet row count before/after and row delta;
- missing rows;
- duplicate IDs and request IDs;
- maximum frontend in-flight count;
- p50, p95, and maximum round-trip latency;
- backend lock wait, Sheet write, Sheet read, total server timing;
- first saturation point, if any.

Pass contract:

- every click is represented at most once;
- row delta equals successful writes;
- zero missing rows;
- zero duplicate IDs;
- zero duplicate request IDs;
- maximum frontend in-flight count is at most 10;
- display never regresses to an older server timestamp;
- no silent retries.

Preserve real failures and saturation evidence.

## Phase 8 — Sheet integrity

Validate all rows created during this task:

- exact header unchanged;
- nonempty unique `id`;
- nonempty unique `requestId`;
- valid server timestamp values;
- nonempty Google subject;
- exact authorized account;
- positive integer client sequence;
- valid client and creation timestamps;
- unauthenticated or denied attempts created zero rows.

Do not record the Spreadsheet ID in GitHub.

## Phase 9 — Google Sites

Use an actual editable Google Sites test page only. Embed `https://134767.github.io/aichat/` and repeat smoke, primary single-write, and incognito-before-sign-in checks.

If no editable Sites target exists, report:

```yaml
google_sites_embed: BLOCKED_MISSING_EDITABLE_SITES_TARGET
```

A generic local iframe is not valid Google Sites evidence.

## Phase 10 — PR and reports

- Keep target PR #3 Draft and unmerged.
- Do not modify the target branch unless a real source defect is proven.
- If a target fix is necessary, make the smallest feature-branch commit, rerun checks, update the pinned preview SHA, redeploy, and retest.
- Add one structured PR #3 comment with preview URL, deployed target SHA, runtime results, stress results, integrity result, and exact blockers.

Write evidence under `evidence/` with task suffix `-002` and replace `outbox/CODEX_REPORT.md`.

Required report schema:

```yaml
task_id: CD35-LCS-2.2.0-AICHAT-PAGES-RUNTIME-MATRIX-002
status: COMPLETE | PARTIAL | BLOCKED
control_task_sha: <sha>
previous_report_commit: 0eeae427c724160570b8fc34b7f6799d3be6f6a4
preview_workflow_commit: <sha|NONE>
preview_url: https://134767.github.io/aichat/
preview_deployed_target_sha: <sha|NOT_DEPLOYED>
preview_pages: PASS|FAIL|BLOCKED
preview_console_errors: <number|NOT_RUN>
target_start_sha: <sha>
target_final_sha: <sha>
target_pr_draft: true
target_pr_merged: false
static_tests: <passed>/<total>|FAIL
credential_scan: PASS|FAIL
gas_runtime: PASS|FAIL|NOT_RUN
isolated_sheet_private: PASS|FAIL|NOT_RUN
primary_account:
  auth: PASS|FAIL|NOT_RUN
  read: PASS|FAIL|NOT_RUN
  single_write: PASS|FAIL|NOT_RUN
  row_delta: <number|NOT_RUN>
  display_matches_sheet: PASS|FAIL|NOT_RUN
incognito_unauthenticated:
  protected_data_exposed: false|true|NOT_RUN
  write_enabled: false|true|NOT_RUN
  row_delta: <number|NOT_RUN>
shared_account: PASS|FAIL|BLOCKED|NOT_RUN
unauthorized_account: PASS|FAIL|BLOCKED|NOT_RUN
reload_account_switch: PASS|FAIL|BLOCKED|NOT_RUN
stress_10:
  successful: <number|NOT_RUN>
  failed: <number|NOT_RUN>
  row_delta: <number|NOT_RUN>
stress_25:
  successful: <number|NOT_RUN>
  failed: <number|NOT_RUN>
  row_delta: <number|NOT_RUN>
stress_100:
  successful: <number|NOT_RUN>
  failed: <number|NOT_RUN>
  row_delta: <number|NOT_RUN>
missing_rows: <number|NOT_RUN>
duplicate_ids: <number|NOT_RUN>
duplicate_request_ids: <number|NOT_RUN>
frontend_max_in_flight: <number|NOT_RUN>
latency_ms:
  p50: <number|NOT_RUN>
  p95: <number|NOT_RUN>
  max: <number|NOT_RUN>
saturation_point: <number|NONE|NOT_RUN>
sheet_integrity: PASS|FAIL|NOT_RUN
google_sites_embed: PASS|FAIL|BLOCKED|NOT_RUN
commits:
  - repository: <owner/repo>
    sha: <sha>
    message: <message>
blockers:
  - <exact blocker or NONE>
next_recommended_action: <one exact action>
```

Commit workflow and final report/evidence as intentional separate commits when practical. Push normally to `aichat/main`.

## Completion gate

Core gate requires live aichat Pages, primary auth/read/write, incognito-before-sign-in, all three stress runs, zero missing/duplicate writes, Sheet integrity PASS, and PR #3 remaining Draft/unmerged. Missing shared, unauthorized, or Google Sites test surfaces remain explicit external blockers but must not prevent primary stress execution.
