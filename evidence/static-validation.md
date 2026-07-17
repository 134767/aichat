# Static validation evidence

- Control task SHA: `eaec89fda49e6242f9a4062b0b3b6844bf0f76a9`
- Target HEAD: `58e7dae27c039096969ed43778d72378c530600f`
- Target branch: `feature/lcs-2.2.0-isolated-runtime-auth-bridge`
- `node --check js/app.js`: PASS
- `npm test`: 4/4 PASS
- GAS `.gs` syntax checks: PASS
- `git diff --check`: PASS
- Runtime configuration placeholders: 0
- Repository `REPLACE_WITH_` matches: 1 expected deployment-workflow guard
- Actual credential-pattern matches after excluding the `.gitignore` guard: 0
- Target working tree after validation: clean
- Draft PR #3: open, Draft, mergeable, not merged

The synchronized target branch already contained the exact public Google Web Client ID and GAS URL required by the active task, so no target source commit was necessary.
