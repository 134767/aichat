# Defect remediation evidence — 002

Two live target defects were proven and fixed on the existing PR #3 feature branch; the PR was not merged or marked ready.

1. Apps Script served the bridge inside an additional host wrapper. The relay now posts through the top-level wrapper while the frontend captures and validates the trusted runtime window and origin. Fix: `9654e71a03b7f0d623aaaba70c38e82f4a5a4347`.
2. The frontend write request used `sequence`, while the backend contract requires positive integer `clientSequence`. The payload mapping and client timing fields were corrected. Fix: `1db7ba259170ec1b70a07c99908a515d64d03573`.

The deployed Apps Script revision was updated after the bridge fix. The Pages bundle was cache-busted and pinned to the final target revision. After a cache-busted page load, the primary write and all 135 stress writes succeeded with no new validation errors.

Diagnostic and cache-remediation commits on the target branch were:

- `395633e42daa3629d74a0eee2eef1dd94190e446` — safe bridge handshake diagnostics.
- `aa24feb597878c53608560ed1d078018c416b218` — serialized safe runtime diagnostics.
- `5583a121305faa2e39b46d04ca6da9c76dead44f` — runtime bundle cache bust.

No private configuration or identifier is reproduced here.
