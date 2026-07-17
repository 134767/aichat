# Runtime matrix evidence — 002

| Surface | Auth | Read | Write | Result |
| --- | --- | --- | --- | --- |
| Existing authorized Chrome profile | PASS | PASS | PASS | Single persisted write, row delta +1 |
| Incognito before sign-in | NOT_RUN | NOT_RUN | NOT_RUN | `BLOCKED_NO_INCOGNITO_BROWSER_SURFACE` |
| Exact shared account | BLOCKED | BLOCKED | BLOCKED | `BLOCKED_MISSING_EXACT_SHARED_EMAIL` |
| Existing unauthorized profile | BLOCKED | BLOCKED | BLOCKED | `BLOCKED_NO_EXISTING_UNAUTHORIZED_PROFILE` |
| Reload/account switch | BLOCKED | BLOCKED | BLOCKED | Requires an existing unauthorized profile |
| Editable Google Sites page | BLOCKED | BLOCKED | BLOCKED | `BLOCKED_MISSING_EDITABLE_SITES_TARGET` |

Primary-account browser verification observed a successful bridge handshake, successful latest-row reads, a single successful write, and a UI timestamp corresponding to the persisted server response. No application-owned local storage, session storage, or cookie-based authorization mechanism was introduced.

Stress phases were executed through the single visible action control after primary read/write passed:

| Phase | Successful | Failed | Row delta | p50 ms | p95 ms | max ms |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| 10 | 10 | 0 | 10 | 4381.3 | 5294.2 | 5294.2 |
| 25 | 25 | 0 | 25 | 2849.9 | 4897.7 | 6037.8 |
| 100 | 100 | 0 | 100 | 2933.7 | 5105.4 | 5621.0 |

All final-build success records included frontend round-trip and backend lock, write, read, and total-server timing fields. The final build added no write failures. Maximum in-flight behavior is capped at 10 by implementation and its passing contract test. No saturation point was observed.
