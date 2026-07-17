# Ephemeral unauthenticated gate evidence — 003

## Fresh context

```yaml
method: newly launched Chrome process plus Playwright browser.newContext()
context_created_for_task: true
storage_state_supplied: false
google_session_cookies_before_navigation: 0
google_session_present: false
context_closed_after_capture: true
browser_closed_after_capture: true
```

The context was created only for task 003 and was closed after evidence capture. It did not reuse the authenticated Chrome profile used for the separate authorized Sheet row-count checks.

## Pre-sign-in frontend gate

```yaml
sign_in_control_visible: true
write_button_enabled: false
protected_timestamp_displayed: false
local_storage_entries: 0
session_storage_entries: 0
indexeddb_databases: 0
indexeddb_auth_records: 0
application_auth_cookies: 0
token_in_url: false
bridge_ready: true
```

Google Identity/FedCM provider messages were recorded separately from application errors. Provider-owned transient state was not counted as application authentication persistence.

## Backend fail-closed proof

```yaml
missing_token_rejected: true
missing_token_result: TOKEN_REQUIRED
malformed_dummy_token_rejected: true
malformed_dummy_token_result: TOKEN_INVALID
protected_record_returned: false
intentional_backend_rejections: 2
```

Both requests used the deployed top-page-to-GAS bridge message contract and the expected parent origin. No real token was used or recorded, and the origin/token checks were not weakened.

## Zero-row-delta proof

```yaml
authorized_read_only_rows_before: 157
authorized_read_only_rows_after: 157
unauthenticated_sheet_row_delta: 0
manual_cleanup_used: false
```

The before/after counts were obtained through a separate authorized read-only browser surface. No write or cleanup action was performed during task 003.

## Final console and network gate

The first diagnostic identified one preview-only missing favicon request. The Pages workflow now injects `<link rel="icon" href="data:,">` into the deployed artifact without modifying the target source. Deployment run `29570857526` succeeded from control commit `a218e75955cd8e7f874a4fe213f71b1b2d94b2b4`.

```yaml
new_application_console_errors: 0
new_unexpected_network_failures: 0
expected_provider_messages: 6
intentional_backend_rejections: 2
```

## Repository and PR verification

```yaml
aichat_pages_target_sha: 1db7ba259170ec1b70a07c99908a515d64d03573
target_working_tree_clean: true
target_static_tests: 5/5 PASS
target_gas_syntax: PASS
runtime_placeholders: 0
tracked_sensitive_files: 0
credential_scan: PASS
target_pr_draft: true
target_pr_state: OPEN
target_pr_merged: false
pr_body_updated: true
pr_comment_url: https://github.com/134767/test/pull/3#issuecomment-5001638969
```

The task 002 primary authorized path, 10/25/100 stress results, and 136-row integrity evidence remain accepted and were not rerun. External shared-account, valid-unauthorized-account, and editable Google Sites surfaces remain unavailable; they do not prevent completion of this focused task.

No private identifier or secret is reproduced in this evidence.
