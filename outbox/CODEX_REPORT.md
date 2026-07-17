```yaml
task_id: CD35-LCS-2.2.0-ISOLATED-RUNTIME-DEPLOY-VERIFY-001
status: PARTIAL
control_task_sha: eaec89fda49e6242f9a4062b0b3b6844bf0f76a9
target_repository: 134767/test
target_branch: feature/lcs-2.2.0-isolated-runtime-auth-bridge
target_start_sha: 58e7dae27c039096969ed43778d72378c530600f
target_final_sha: 58e7dae27c039096969ed43778d72378c530600f
target_working_tree_clean: true
target_pr: 3
target_pr_draft: true
target_pr_merged: false
gas_exec_reachable: PASS
gas_script_properties: PASS
isolated_sheet_private: PASS
static_tests: 4/4
credential_scan: PASS
pages_direct: BLOCKED
local_direct: PASS
google_sites_embed: NOT_RUN
primary_account: NOT_RUN
shared_account: BLOCKED
incognito_unauthenticated: NOT_RUN
unauthorized_account: NOT_RUN
reload_account_switch: NOT_RUN
stress_10: NOT_RUN
stress_25: NOT_RUN
stress_100: NOT_RUN
missing_rows: NOT_RUN
duplicate_request_ids: NOT_RUN
p50_ms: NOT_RUN
p95_ms: NOT_RUN
max_ms: NOT_RUN
saturation_point: NOT_RUN
commits:
  - sha: 58e7dae27c039096969ed43778d72378c530600f
    message: "chore: point isolated runtime to latest GAS deployment (pre-existing synchronized target HEAD)"
blockers:
  - BLOCKED_NO_BRANCH_PREVIEW: the repository workflow verifies the feature ref but the github-pages environment rejects this branch.
  - BLOCKED_MISSING_EXACT_SHARED_EMAIL: project sharing and non-secret configuration expose only the owner.
  - Identity and stress matrices require an origin-valid preview plus actual primary, incognito, shared, and unauthorized browser profiles.
next_recommended_action: Implement a separate branch-preview workflow and preview URL for the feature branch without changing the production Pages root, then authorize that preview origin and run the full identity and stress matrix.
```

## Repository and static evidence

The control repository was synchronized first at `eaec89fda49e6242f9a4062b0b3b6844bf0f76a9`. The target feature branch fast-forwarded cleanly to `58e7dae27c039096969ed43778d72378c530600f`; no unknown local work existed.

`node --check js/app.js`, all GAS syntax checks, and `git diff --check` passed. `npm test` passed all four isolated-runtime contracts. Runtime placeholders are absent; the only repository `REPLACE_WITH_` match is the expected workflow guard. Credential scanning found no actual credential values.

Detailed evidence: [static-validation.md](../evidence/static-validation.md).

## GAS and Sheet evidence

The specified Apps Script project and deployment were resolved under `fjulibrs@gmail.com`. All six Script Property keys exist. The first bridge request exposed a configuration defect: the allowed parent value included `/test/`, so the backend correctly failed closed with `ORIGIN_DENIED`. Only the non-private allowed-origin property was corrected to `https://134767.github.io`; the same bridge URL then loaded successfully with zero console errors.

The deployment itself was also misconfigured to execute as the accessing user and require a signed-in Google account. The same deployment was updated in place to execute as the deployer and allow everyone. A no-cookie HTTP request then returned 200 with the expected bridge title and no Google login redirect, confirming anonymous bridge reachability without creating a duplicate deployment.

The isolated Sheet is owner-only and contains the `timestamp_log` tab with the exact required nine-column header. Private Script, deployment, and Spreadsheet identifiers are omitted from this report and evidence.

Detailed evidence: [runtime-validation.md](../evidence/runtime-validation.md).

## Browser, Pages, and stress evidence

The local frontend rendered one Google sign-in control, one timestamp container, and one hidden bridge iframe. It had zero local/session storage entries, zero cookies, zero console errors, and returned no protected timestamp before authorization.

Workflow run https://github.com/134767/test/actions/runs/29561837671 verified the exact feature SHA, but the deploy job was rejected by the existing `github-pages` environment branch policy. The current production Pages root is a different application. Per the active task, production Pages settings were not changed.

Because the bridge enforces the exact GitHub origin, localhost cannot complete identity dispatch. No branch preview and no separate incognito/profile surfaces were available, so primary/shared/incognito/unauthorized/reload-switch and 10/25/100 stress tests remain NOT_RUN. No result was inferred or retried into a false PASS.

Detailed evidence: [browser-and-pages.md](../evidence/browser-and-pages.md).

## GitHub handoff

PR #3 remains Draft, open, mergeable, and unmerged. Structured runtime evidence and blockers were posted at https://github.com/134767/test/pull/3#issuecomment-4999960897, with the deployment-mode correction recorded at https://github.com/134767/test/pull/3#issuecomment-4999996805.
