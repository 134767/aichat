# ACTIVE TASK PACKET

```yaml
task_id: CD35-LCS-2.2.0-PUBLIC-ACCOUNT-SITES-SHARED-MATRIX-004
mode: CLOUD_DIRECTOR_3_5
issuer: ChatGPT Cloud Director
executor: Codex
status: READY
priority: P1
control_repository: 134767/aichat
target_repository: 134767/test
target_branch: feature/lcs-2.2.0-isolated-runtime-auth-bridge
target_expected_sha: 1db7ba259170ec1b70a07c99908a515d64d03573
target_pr: 3
preview_url: https://134767.github.io/aichat/
public_google_account: fjulibrs@gmail.com
accepted_report_commit: afa8de077db42f1d16c6c1fac13145ed3dc72bae
```

## Objective

Complete only the remaining public-account test surfaces:

1. Resolve the exact shared operational Google account from the `fjulibrs@gmail.com` work environment.
2. Create an isolated temporary Google Sites test page under `fjulibrs@gmail.com`, embed the existing preview, and validate runtime behavior.

Do not test with or inspect the user's private Google identities. Do not use `134767@gapp.fju.edu.tw`, `134767@mail.fju.edu.tw`, or any other personal account as an unauthorized or shared-account candidate.

Do not merge PR #3. Do not modify target main. Do not rerun 10/25/100 stress tests.

## Accepted baseline

- Primary authorized auth/read/write: PASS.
- Stress 10/25/100: PASS.
- Sheet integrity: PASS.
- Fresh unauthenticated gate: PASS.
- Missing and malformed tokens rejected.
- Unauthenticated Sheet row delta: 0.
- Target SHA remains `1db7ba259170ec1b70a07c99908a515d64d03573`.

## Constraints

- Operate only in the authenticated `fjulibrs@gmail.com` browser/Drive/Apps Script environment.
- Never guess an account email.
- Never expose or commit passwords, cookies, tokens, account subjects, Spreadsheet IDs, Script IDs, deployment IDs, browser profile data, or private configuration values.
- Do not add an account to the allowlist unless exact evidence proves it is the intended operational shared account and the task requires temporary authorization for testing.
- Any temporary Google Site must contain only the isolated preview and must not expose production LCS data.
- Prefer deleting or unpublishing the temporary Site after evidence capture.
- Source changes require proof of a real defect. Otherwise create no target commit.

## Test A — exact shared operational account resolution

Inspect only non-secret evidence available under `fjulibrs@gmail.com`:

- Apps Script project collaborators and deployment ownership.
- Existing Google Drive sharing metadata for LCS-related files/folders.
- Existing Google Sites collaborators under the public-account environment.
- Existing non-secret LCS configuration and browser account chooser entries belonging to the public work environment.

Required outcome:

```yaml
shared_account_email: <exact email|UNRESOLVED>
resolution_evidence: <non-secret source description>
```

If unresolved, return `BLOCKED_MISSING_EXACT_SHARED_EMAIL`. Never infer from display names.

When resolved and the account is already present in the server allowlist, test auth/read/one write and exact row delta +1.

When resolved but absent from the allowlist, do not silently add it. Report the mismatch and wait for Cloud Director authorization unless existing project policy clearly identifies it as an authorized operational account.

## Test B — isolated Google Sites embed

Under `fjulibrs@gmail.com`, create or use a temporary isolated Google Site dedicated to this test.

Embed:

```text
https://134767.github.io/aichat/
```

Validate from the published or previewed Sites page:

```yaml
site_surface_loaded: true
iframe_preview_loaded: true
visible_write_button_count: 1
visible_timestamp_container_count: 1
bridge_ready: true
primary_google_sign_in: PASS
initial_read: PASS
single_write: PASS
sheet_row_delta: 1
ui_matches_persisted_response: PASS
new_application_console_errors: 0
new_unexpected_network_failures: 0
```

Confirm the Sites wrapper does not alter the effective GitHub parent origin used by the GAS bridge. If embedding is blocked by browser, OAuth, CSP, X-Frame-Options, Sites restrictions, or third-party identity behavior, preserve exact evidence and do not claim PASS.

After evidence capture, unpublish or delete the temporary Site when technically possible. Record cleanup status without exposing private Site identifiers.

## Test C — final repository verification

Confirm:

```yaml
target_head_sha: 1db7ba259170ec1b70a07c99908a515d64d03573
target_pr_draft: true
target_pr_open: true
target_pr_merged: false
target_working_tree_clean: true
credential_scan: PASS
```

Do not create a target commit unless Test B proves a real source defect.

## Delivery

Replace `outbox/CODEX_REPORT.md` and add `evidence/public-account-sites-shared-004.md`.

Top-level report schema:

```yaml
task_id: CD35-LCS-2.2.0-PUBLIC-ACCOUNT-SITES-SHARED-MATRIX-004
status: COMPLETE | PARTIAL | BLOCKED
control_task_sha: <sha>
public_google_account: fjulibrs@gmail.com
shared_account_email: <exact email|UNRESOLVED>
shared_account_test: PASS|FAIL|BLOCKED|NOT_RUN
google_sites_embed: PASS|FAIL|BLOCKED|NOT_RUN
google_sites_primary_auth: PASS|FAIL|NOT_RUN
google_sites_read: PASS|FAIL|NOT_RUN
google_sites_single_write: PASS|FAIL|NOT_RUN
google_sites_row_delta: <number|NOT_RUN>
google_sites_console_errors: <number|NOT_RUN>
google_sites_network_failures: <number|NOT_RUN>
temporary_site_cleanup: DELETED|UNPUBLISHED|RETAINED_WITH_REASON|NOT_CREATED
target_head_sha: <sha>
target_pr_draft: true|false
target_pr_merged: false|true
blockers:
  - <exact blocker or NONE>
next_recommended_action: <one exact action>
```

Add one concise PR #3 evidence comment. Keep PR Draft and unmerged.

Commit and push only the report/evidence and any control-repository metadata genuinely needed. Do not modify this task file or `state/PROJECT_STATE.yaml`.

## Completion rule

`COMPLETE` requires Google Sites embed/auth/read/single-write/zero-console validation to pass and the shared account either to pass or to be conclusively documented as nonexistent in the available public-account environment. Otherwise return PARTIAL or BLOCKED with exact evidence.