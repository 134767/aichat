# Browser and Pages evidence

## Local direct

Served the target checkout with `python -m http.server 5500` and opened `http://127.0.0.1:5500/`.

- Visible Google sign-in control: 1
- Timestamp display container: 1
- Hidden bridge iframe: 1
- `localStorage` entries: 0
- `sessionStorage` entries: 0
- Cookie length: 0
- Protected timestamp displayed before authorization: no (`—`)
- Console errors/warnings: 0

Local identity dispatch cannot complete by design: the bridge accepts only the exact GitHub origin, while a local parent has origin `http://127.0.0.1:5500`.

## GitHub Pages

- Workflow run: https://github.com/134767/test/actions/runs/29561837671
- Workflow ref/HEAD: feature branch at `58e7dae27c039096969ed43778d72378c530600f`
- Verify job: PASS
- Deploy job: rejected because the feature branch is not allowed by the `github-pages` environment branch policy
- Existing Pages root: a different application, not this isolated runtime
- Result: `BLOCKED_NO_BRANCH_PREVIEW`

No production Pages source was changed. Identity/read/write/stress tests were not run because doing so requires an origin-valid branch preview and actual separate browser profiles. No failed call was silently retried.

## GitHub delivery

- PR evidence comment: https://github.com/134767/test/pull/3#issuecomment-4999960897
- GAS deployment correction addendum: https://github.com/134767/test/pull/3#issuecomment-4999996805
- PR #3 remains Draft and unmerged.
