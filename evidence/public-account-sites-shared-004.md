# Task 004 public-account Sites and shared-account evidence

## Scope and identity boundary

- Browser, Drive, Apps Script, Sheet, and Sites activity used only the account banner `Google 帳戶： 輔大圖書館讀服組 (fjulibrs@gmail.com)`.
- No private Google identity was opened, enumerated, inspected, or used as an account candidate.
- No private identifier, credential, token, cookie, account subject, Spreadsheet ID, Script ID, deployment ID, Site identifier, or profile path was retained.

## Shared operational account resolution

- The LCS isolated-runtime database and Apps Script project were visible in the public account's Drive and were owned by the public account.
- The selected Apps Script project's visible access metadata classified it as a private file and did not disclose a collaborator email.
- The public-account share dialog returned `很抱歉，目前無法共用。請稍後再試。` before additional sharing metadata was available.
- No exact second operational email was available from the inspected public-work evidence. Display names were not treated as email evidence.

Result:

```yaml
shared_account_email: UNRESOLVED
resolution_evidence: "Public-account Drive ownership/access metadata disclosed no exact collaborator email; the share dialog was unavailable."
shared_account_test: BLOCKED
blocker: BLOCKED_MISSING_EXACT_SHARED_EMAIL
```

## Google Sites matrix

The public account created an isolated, unpublished temporary Google Site containing only the existing preview. The first embed reproduced a bridge readiness failure. Source inspection tied the failure to `Bridge.html` sending to and accepting only `top`: in a Sites nesting chain, `top` is the Sites wrapper rather than the GitHub application.

The minimal target fix:

- Captures only actual ancestor `WindowProxy` objects.
- Relays bridge readiness and responses toward those ancestors using the exact configured GitHub `targetOrigin`; non-matching ancestors do not receive the message payload.
- Accepts requests only when `event.source` is an actual ancestor and `event.origin` exactly matches the allowed GitHub origin.
- Retains the frontend's existing exact googleusercontent response-origin and captured-source validation.

Target verification:

```text
target branch: feature/lcs-2.2.0-isolated-runtime-auth-bridge
target head: 90b298ad94193eb6059794f265224b0911159a55
node application syntax: PASS
npm contract suite: PASS (5/5)
GAS .gs syntax: PASS
target working tree: CLEAN
target remote synchronization: PASS
```

The authorized GAS project was pushed, a new immutable version was created, and the active public deployment was updated without retaining or exposing any private deployment identifier. Pages pinned the exact target commit and [workflow run 29577995248](https://github.com/134767/aichat/actions/runs/29577995248) succeeded.

Final live matrix:

```yaml
site_surface_loaded: true
iframe_preview_loaded: true
visible_write_button_count: 1
visible_timestamp_container_count: 1
bridge_ready: true
primary_google_sign_in: PASS
initial_read: PASS
single_write: PASS
sheet_data_rows_before: 157
sheet_data_rows_after: 158
sheet_row_delta: 1
ui_matches_persisted_response: PASS
new_application_console_errors: 0
new_unexpected_network_failures: 0
temporary_site_cleanup: DELETED
```

The single write had client sequence `1`; the persisted server timestamp and the embedded UI's localized timestamp matched. Provider-owned Google Identity/FedCM diagnostics observed before the final write interval were categorized separately from application errors; the measured final interaction interval added no console error or network failure.

## Repository and PR state

```yaml
target_pr: 3
target_pr_draft: true
target_pr_open: true
target_pr_merged: false
target_base_branch: main
pr_evidence_comment: https://github.com/134767/test/pull/3#issuecomment-5003018752
stress_10_25_100_rerun: false
credential_scan: PASS
current_task_blob: f6dda2c98bfb9eae6d1aca3313b1b1196f6478a9
project_state_blob: 22bf3cd9302c28784fd43daf0118ce99741be743
```

The target commit exists only on the existing feature branch because the live Sites matrix proved a source defect. PR #3 was not merged or marked ready, and target `main` was not modified.
