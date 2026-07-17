# ACTIVE TASK PACKET

```yaml
task_id: CD35-LCS-2.2.0-ISOLATED-RUNTIME-DEPLOY-VERIFY-001
mode: CLOUD_DIRECTOR_3_5
issuer: ChatGPT Cloud Director
executor: Codex
control_repository: 134767/aichat
status: READY
priority: P0
```

## Objective

Take over execution of the existing LCS 2.2.0 isolated runtime authentication test. Verify the current GitHub branch and deployed GAS endpoint, complete any missing runtime configuration, deploy the GitHub test frontend without modifying target `main`, execute identity/read/write/stress tests, collect evidence, and report through this control repository.

## Target systems

```yaml
target_repository: 134767/test
target_clone_ssh: git@github.com:134767/test.git
target_base_branch: main
target_working_branch: feature/lcs-2.2.0-isolated-runtime-auth-bridge
target_draft_pr: 3
target_pages_url: https://134767.github.io/test/
gas_project_name: LCS 2.2.0 Isolated Runtime Test
gas_deployer: fjulibrs@gmail.com
gas_exec_url: https://script.google.com/macros/s/AKfycbxZo4FnF-DUJ66fTDKC2F6VQQ8WVA6G5UyBGNlcoKeTJlEehVob5mK1ZPVfcNdfnlfYjg/exec
expected_google_client_id: 969836734244-25r4usq6vv9b6vd70u2q623fmehcb55a.apps.googleusercontent.com
isolated_sheet_title: LCS 2.2.0 Isolated Runtime Test DB
isolated_sheet_tab: timestamp_log
```

## Non-negotiable constraints

- Never force push.
- Never modify target `main` directly.
- Never merge PR #3.
- Never use a production LCS Sheet.
- Never commit Spreadsheet ID, Apps Script ID, OAuth client secret, private key, access token, refresh token, `.clasp.json`, or `.clasprc.json`.
- Preserve all unknown local work.
- Every protected read and write must verify Google ID Token and server-side exact allowlist.
- Never trust client-provided email, role, or authorization flags.
- Do not claim PASS without runtime evidence.
- Keep the visible frontend to one timestamp-write button and one timestamp display container.

## Phase 1 — Control repository synchronization

Clone or update this repository first:

```powershell
$ErrorActionPreference = 'Stop'
$controlCandidates = @(
  'C:\Users\v5990\Desktop\githhub\aichat',
  'C:\Users\帛勳\Desktop\githhub\aichat'
)
$controlRepo = $controlCandidates | Where-Object { Test-Path $_ } | Select-Object -First 1
if (-not $controlRepo) {
  $controlRepo = $controlCandidates[0]
  git clone git@github.com:134767/aichat.git $controlRepo
}
Set-Location $controlRepo
git fetch origin --prune
git switch main
git pull --ff-only origin main
Get-Content inbox/CURRENT_TASK.md -Raw
```

Record the current control-repository HEAD as `control_task_sha`.

## Phase 2 — Target repository synchronization

```powershell
$targetCandidates = @(
  'C:\Users\v5990\Desktop\githhub\test',
  'C:\Users\帛勳\Desktop\githhub\test'
)
$targetRepo = $targetCandidates | Where-Object { Test-Path $_ } | Select-Object -First 1
if (-not $targetRepo) {
  $targetRepo = $targetCandidates[0]
  git clone git@github.com:134767/test.git $targetRepo
}
Set-Location $targetRepo
git fetch origin --prune
$branch = 'feature/lcs-2.2.0-isolated-runtime-auth-bridge'
if (git show-ref --verify --quiet "refs/heads/$branch") {
  git switch $branch
} else {
  git switch --track "origin/$branch"
}
git pull --ff-only origin $branch
git status --short --branch
```

If tracked or untracked files exist, inspect and preserve them. Do not reset.

## Phase 3 — Configuration audit

Verify `js/runtime-config.js` contains exactly:

```javascript
googleClientId: '969836734244-25r4usq6vv9b6vd70u2q623fmehcb55a.apps.googleusercontent.com'
gasWebAppUrl: 'https://script.google.com/macros/s/AKfycbxZo4FnF-DUJ66fTDKC2F6VQQ8WVA6G5UyBGNlcoKeTJlEehVob5mK1ZPVfcNdfnlfYjg/exec'
parentOrigin: 'https://134767.github.io'
bridgeChannel: 'LCS_2_2_0_ISOLATED_BRIDGE'
requestTimeoutMs: 30000
maxInFlight: 10
```

Run:

```powershell
git grep -n 'REPLACE_WITH_' -- .
git grep -n -E 'client_secret|private_key|refresh_token|ya29\.|1//[A-Za-z0-9_-]+' -- .
node --check js/app.js
npm test
Get-ChildItem gas-lcs-2.2.0-isolated -Filter *.gs | ForEach-Object {
  $tmp = Join-Path $env:TEMP ('lcs-gas-' + $_.BaseName + '.js')
  Copy-Item $_.FullName $tmp -Force
  node --check $tmp
}
git diff --check
```

If runtime config still points to another GAS URL, update only `gasWebAppUrl`, rerun all checks, commit, and push to the working branch.

## Phase 4 — GAS deployment validation

Using the authenticated `fjulibrs@gmail.com` Apps Script/clasp environment:

1. Confirm the deployed project is `LCS 2.2.0 Isolated Runtime Test`.
2. Confirm deployment is `executeAs: USER_DEPLOYING` and `access: ANYONE_ANONYMOUS`.
3. Confirm Script Properties exist without exposing their values:
   - `LCS_TEST_SPREADSHEET_ID`
   - `LCS_TEST_SHEET_NAME`
   - `LCS_TEST_GOOGLE_CLIENT_ID`
   - `LCS_TEST_AUTHORIZED_EMAILS`
   - `LCS_TEST_ALLOWED_PARENT_ORIGIN`
   - `LCS_TEST_BRIDGE_CHANNEL`
4. Confirm the isolated Sheet is private and owned by `fjulibrs@gmail.com`.
5. Confirm `timestamp_log` has the exact nine-column schema.
6. Open the bridge endpoint:

```text
https://script.google.com/macros/s/AKfycbxZo4FnF-DUJ66fTDKC2F6VQQ8WVA6G5UyBGNlcoKeTJlEehVob5mK1ZPVfcNdfnlfYjg/exec?mode=bridge&parentOrigin=https%3A%2F%2F134767.github.io&channel=LCS_2_2_0_ISOLATED_BRIDGE
```

A visually blank bridge is acceptable. HTTP, authorization, script, or HTML failure is not acceptable.

## Phase 5 — GitHub deployment

Do not merge target PR #3.

Deploy the working branch using the safest existing repository-supported path. Prefer a GitHub Actions `workflow_dispatch` with branch/ref support or a separate preview deployment. Do not point the production Pages root at an unverified branch unless the active repository workflow already intentionally deploys this branch.

If repository Pages can only deploy from `main`, do not merge. Instead:

- use a local HTTP server for direct runtime tests;
- validate the GAS bridge and identity flow locally;
- report GitHub Pages preview as `BLOCKED_NO_BRANCH_PREVIEW`;
- propose the minimal workflow change needed for branch preview in the report.

## Phase 6 — Browser identity matrix

Test with actual browser profiles. Do not infer results.

### A. Primary authorized account

Account: `fjulibrs@gmail.com`

Expected:

- Google ID Token acquired.
- Server allowlist accepted.
- Initial latest-record read succeeds.
- One timestamp write succeeds.
- Sheet row delta is exactly 1.
- Displayed timestamp matches persisted row.

### B. Shared authorized account

Resolve the exact shared operational account from existing non-secret configuration or project sharing metadata. Never infer an email.

If unresolved, report `BLOCKED_MISSING_EXACT_SHARED_EMAIL` and continue.

Expected when resolved:

- Authentication succeeds.
- Allowlist succeeds.
- Read succeeds.
- One write succeeds.
- Sheet row delta is exactly 1.
- Direct Sheet sharing is not required.

### C. Incognito, no Google session

Expected before sign-in:

- No protected data returned.
- Write button disabled.
- Sheet row delta 0.
- Google sign-in control available.

### D. Valid but unauthorized Google account

Expected:

- Google identity itself is valid.
- GAS returns `ACCESS_DENIED`.
- No protected data returned.
- Write remains disabled.
- Sheet row delta 0.

### E. Reload and account switch

Authenticate with the primary account, write once, reload, then switch to an unauthorized account and reload.

Expected:

- No token persisted in localStorage, IndexedDB, cookies, or URL.
- No protected data shown under the unauthorized account.
- New write denied.
- Sheet row delta 0 after denial.

## Phase 7 — Read/write and stress tests

Use the browser harness:

```javascript
window.LCS_TEST_HARNESS.burst(count, intervalMs)
window.LCS_TEST_HARNESS.waitForIdle(timeoutMs)
window.LCS_TEST_HARNESS.snapshot()
```

Run sequentially on a clean evidence baseline:

```javascript
LCS_TEST_HARNESS.burst(10, 250)
await LCS_TEST_HARNESS.waitForIdle(180000)

LCS_TEST_HARNESS.burst(25, 0)
await LCS_TEST_HARNESS.waitForIdle(180000)

LCS_TEST_HARNESS.burst(100, 0)
await LCS_TEST_HARNESS.waitForIdle(600000)
```

For each run capture:

- requested
- successful
- failed
- Sheet row delta
- missing rows
- duplicate IDs
- duplicate request IDs
- maximum frontend in-flight count
- frontend p50/p95/max round-trip latency
- backend lock wait, Sheet write, Sheet read, and total server time
- exact saturation point if any

Never silently retry failed calls until the result looks successful.

## Phase 8 — Target repository publication

When code/config changes are required:

1. Run all static checks again.
2. Commit intentionally on `feature/lcs-2.2.0-isolated-runtime-auth-bridge`.
3. Push normally.
4. Keep PR #3 Draft.
5. Add a structured PR comment with runtime evidence and blockers.
6. Do not merge.

## Phase 9 — Control-repository report

Replace or create `outbox/CODEX_REPORT.md` in `134767/aichat` with this exact top-level schema:

```yaml
task_id: CD35-LCS-2.2.0-ISOLATED-RUNTIME-DEPLOY-VERIFY-001
status: COMPLETE | PARTIAL | BLOCKED
control_task_sha: <sha>
target_repository: 134767/test
target_branch: feature/lcs-2.2.0-isolated-runtime-auth-bridge
target_start_sha: <sha>
target_final_sha: <sha>
target_working_tree_clean: true|false
target_pr: 3
target_pr_draft: true
target_pr_merged: false
gas_exec_reachable: PASS|FAIL|NOT_RUN
gas_script_properties: PASS|FAIL|NOT_RUN
isolated_sheet_private: PASS|FAIL|NOT_RUN
static_tests: <passed>/<total>|FAIL
credential_scan: PASS|FAIL
pages_direct: PASS|FAIL|BLOCKED|NOT_RUN
local_direct: PASS|FAIL|NOT_RUN
google_sites_embed: PASS|FAIL|NOT_RUN
primary_account: PASS|FAIL|NOT_RUN
shared_account: PASS|FAIL|BLOCKED|NOT_RUN
incognito_unauthenticated: PASS|FAIL|NOT_RUN
unauthorized_account: PASS|FAIL|NOT_RUN
reload_account_switch: PASS|FAIL|NOT_RUN
stress_10: PASS|FAIL|NOT_RUN
stress_25: PASS|FAIL|NOT_RUN
stress_100: PASS|FAIL|NOT_RUN
missing_rows: <number|NOT_RUN>
duplicate_request_ids: <number|NOT_RUN>
p50_ms: <number|NOT_RUN>
p95_ms: <number|NOT_RUN>
max_ms: <number|NOT_RUN>
saturation_point: <number|NONE|NOT_RUN>
commits:
  - sha: <sha>
    message: <message>
blockers:
  - <exact blocker or NONE>
next_recommended_action: <one exact action>
```

Below the YAML include concise evidence sections with commands, URLs, test snapshots, row-count evidence, console errors, and PR comment URL. Redact private identifiers.

Commit and push the control report:

```powershell
Set-Location $controlRepo
git pull --ff-only origin main
git add outbox/CODEX_REPORT.md evidence
git commit -m "report: LCS isolated runtime deployment verification"
git push origin main
```

Do not alter `inbox/CURRENT_TASK.md` or `state/PROJECT_STATE.yaml`. Only Cloud Director may replace those files.

## Completion condition

The task is COMPLETE only when all available account tests and all 10/25/100 stress tests have runtime evidence, denied users created zero rows, no missing or duplicate writes exist, and PR #3 remains unmerged.

If browser access, Google Cloud OAuth configuration, exact shared account, or branch-preview deployment is unavailable, report PARTIAL/BLOCKED with exact evidence and complete every unaffected phase.
