# Identity scope correction

The Google connector available to ChatGPT is not the work-account environment used by Codex for GAS runtime testing.

Therefore:

- Connector search results must not be used to identify the shared work account.
- Existing Codex runtime evidence remains valid because Codex tested inside the separate work-account browser environment.
- The remaining shared-account test requires an exact address supplied or verified inside that work environment.
- No personal account identifier is recorded in this public repository.
