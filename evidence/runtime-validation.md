# Runtime validation evidence

## GAS

- Authenticated clasp account: `fjulibrs@gmail.com`
- Exact project title resolved once: `LCS 2.2.0 Isolated Runtime Test`
- Expected deployment resolved once; Script ID and deployment ID are redacted.
- Deployment UI initially showed the wrong runtime mode: execute as the accessing user and allow only signed-in Google accounts.
- Updated the same deployment in place to execute as `fjulibrs@gmail.com` and allow `Everyone`; no duplicate deployment was created.
- Apps Script reported the deployment update succeeded.
- A fresh no-cookie HTTP request returned status 200, contained the expected bridge title, and did not redirect to Google sign-in. This confirms `ANYONE_ANONYMOUS` behavior.
- Source manifest contract: `ANYONE_ANONYMOUS`, `USER_DEPLOYING`.
- Six required Script Property keys were present.
- Initial bridge request failed closed with `ORIGIN_DENIED` because `LCS_TEST_ALLOWED_PARENT_ORIGIN` incorrectly contained a URL path.
- Corrected only that non-private property to the exact origin `https://134767.github.io`.
- Repeated bridge request: PASS; page title `LCS 2.2.0 GAS Bridge`; console errors 0.

## Isolated Sheet

- Title: `LCS 2.2.0 Isolated Runtime Test DB`
- Owner account: `fjulibrs@gmail.com`
- Sharing UI: owner-only / private
- Tab: `timestamp_log`
- Header: `id`, `serverTimestampIso`, `serverTimestampMs`, `authorizedEmail`, `googleSub`, `requestId`, `clientSequence`, `clientClickedAtIso`, `createdAtIso`
- No Spreadsheet ID is recorded in this evidence.

## Shared account resolution

- Apps Script sharing metadata contained exactly one principal: the owner.
- Local non-secret configuration contained no exact shared operational email.
- Result: `BLOCKED_MISSING_EXACT_SHARED_EMAIL`.
