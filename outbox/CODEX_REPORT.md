# Codex execution report

```yaml
task_id: CD35-LCS-2.2.0-AICHAT-PAGES-RUNTIME-MATRIX-002
status: PARTIAL
control_task_sha: 5255a5842f946d7f2055f28e1be70a23f366f02b
previous_report_commit: 0eeae427c724160570b8fc34b7f6799d3be6f6a4
preview_workflow_commit: 8c7d5147e771040e7f988160b692607c159eb164
preview_url: https://134767.github.io/aichat/
preview_deployed_target_sha: 1db7ba259170ec1b70a07c99908a515d64d03573
preview_pages: PASS
preview_console_errors: 10
target_start_sha: 58e7dae27c039096969ed43778d72378c530600f
target_final_sha: 1db7ba259170ec1b70a07c99908a515d64d03573
target_pr_draft: true
target_pr_merged: false
static_tests: 5/5
credential_scan: PASS
gas_runtime: PASS
isolated_sheet_private: PASS
primary_account:
  auth: PASS
  read: PASS
  single_write: PASS
  row_delta: 1
  display_matches_sheet: PASS
incognito_unauthenticated:
  protected_data_exposed: NOT_RUN
  write_enabled: NOT_RUN
  row_delta: NOT_RUN
shared_account: BLOCKED
unauthorized_account: BLOCKED
reload_account_switch: BLOCKED
stress_10:
  successful: 10
  failed: 0
  row_delta: 10
stress_25:
  successful: 25
  failed: 0
  row_delta: 25
stress_100:
  successful: 100
  failed: 0
  row_delta: 100
missing_rows: 0
duplicate_ids: 0
duplicate_request_ids: 0
frontend_max_in_flight: 10
latency_ms:
  p50: 2933.7
  p95: 5105.4
  max: 5621
saturation_point: NONE
sheet_integrity: PASS
google_sites_embed: BLOCKED
commits:
  - repository: 134767/aichat
    sha: 8c7d5147e771040e7f988160b692607c159eb164
    message: "ci: deploy isolated LCS preview from pinned target SHA"
  - repository: 134767/aichat
    sha: 1716b014fc2bdd593c0cff1af6494ec4ffbec5cf
    message: "ci: deploy write payload contract fix"
  - repository: 134767/test
    sha: 9654e71a03b7f0d623aaaba70c38e82f4a5a4347
    message: "fix: relay GAS bridge through Apps Script wrapper"
  - repository: 134767/test
    sha: 5583a121305faa2e39b46d04ca6da9c76dead44f
    message: "fix: bust cached isolated runtime bundle"
  - repository: 134767/test
    sha: 1db7ba259170ec1b70a07c99908a515d64d03573
    message: "fix: map write payload to backend contract"
blockers:
  - BLOCKED_NO_INCOGNITO_BROWSER_SURFACE
  - BLOCKED_MISSING_EXACT_SHARED_EMAIL
  - BLOCKED_NO_EXISTING_UNAUTHORIZED_PROFILE
  - BLOCKED_MISSING_EDITABLE_SITES_TARGET
next_recommended_action: "Provide an incognito browser surface, then rerun the pre-sign-in gate against the existing preview without changing or merging PR #3."
```

## Outcome

GitHub Pages deployment run [29564616027](https://github.com/134767/aichat/actions/runs/29564616027) completed successfully and serves the pinned target revision. The authorized primary-account path passed authentication, initial read, one write, and persisted-display verification.

The exact stress phases completed with 10/10, 25/25, and 100/100 successful writes. The Sheet gained exactly 136 rows including the primary write; generated IDs and request IDs were unique, client sequence was contiguous from 1 through 136, and server timestamps were nondecreasing. No saturation or new write error occurred.

The status is `PARTIAL` solely because the required incognito-before-sign-in core gate could not be run on the available Chrome surface. The shared-account, unauthorized-account, and Google Sites rows remain explicit external blockers. PR #3 remains Draft, open, and unmerged.

The ten recorded console errors are session-level diagnostics accumulated across cache/remediation cycles, including Google identity/FedCM noise and two preserved pre-fix validation failures. The final cache-busted build produced 136 successful write metrics and zero new write failures.

No credential, token, private deployment identifier, Spreadsheet ID, Script ID, OAuth client identifier, or account subject is included in this report or its evidence.
