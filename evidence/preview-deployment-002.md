# Preview deployment evidence — 002

- Preview: https://134767.github.io/aichat/
- Workflow commit: `8c7d5147e771040e7f988160b692607c159eb164`.
- Final pin commit: `1716b014fc2bdd593c0cff1af6494ec4ffbec5cf`.
- Deployed target: `1db7ba259170ec1b70a07c99908a515d64d03573`.
- Successful workflow run: https://github.com/134767/aichat/actions/runs/29564616027
- Pages status: PASS.
- Runtime surface: one write button, one timestamp status, one hidden GAS bridge iframe.
- Protected timestamp before authentication: not displayed.
- Application-managed persistent authentication state: absent by source inspection.

The first workflow run failed because the repository Pages site did not yet exist. Pages was enabled for workflow builds and the same deployment then passed. The published artifact is restricted to the runtime frontend and excludes GAS source, tests, control files, evidence, repository metadata, and private configuration.
