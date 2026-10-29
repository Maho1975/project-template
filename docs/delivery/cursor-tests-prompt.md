Cursor test run for <OWNER>/<REPO>: <TEST MODEL>, Fast off. No review, no code changes, no merge. This is the only test run on a PR; the reviewer does not repeat it.

Accept only a `test: cursor` label-ADDED event on an open, non-draft, same-repository PR. Fetch its current head SHA and base. Post a start comment: role "test run", model, session ID, the Cursor run URL, the full head SHA and the scope you chose (below).

## Scope: run what the change needs

List the changed files with `git diff --name-only $(git merge-base origin/<base> <head>) <head>`. Pick the first scope that matches. Any changed file that isn't covered by scopes 2–4 means full.

1. **Full.** Any change to dependency manifests or lockfiles, build, test or type-check config, database config, migrations, scripts, `.github/`, or auth and session code: <FULL-SCOPE PATHS>. Any file outside <APP PATHS>, <TOOLING PATHS>, `docs/` and top-level `*.md` also gets this scope, and so does a PR labelled `tests: full`. Run the full `<TEST COMMAND>`, then <PRODUCTION-BUILD CHECK, or delete>.
2. **App.** Any other change under <APP PATHS>: run the full `<TEST COMMAND>`. Add <PRODUCTION-BUILD CHECK> when <HIGH-RISK APP PATHS> changed.
3. **Tooling.** Changes only under <TOOLING PATHS>, plus docs: run `<TOOLING TEST COMMAND>`.
4. **Docs.** Only `*.md`, `docs/` or `AGENTS.md` changed: no run is needed. Still check that no file outside those paths changed.

For scopes 1 and 2, check out that exact SHA in this cloud workspace. Set up the test environment: <SETUP STEPS, e.g. throwaway database with fictional data>. Run `<INSTALL COMMAND>`. If a test server's port is busy, use a free port through <PORT VARIABLES>; a busy port is never a failure. Never use real data, secrets or private services, and never change files or push.

## Result

Post a PR comment whose very first line is exactly `TESTS PASS for <full 40-character head SHA>` only when every step in the chosen scope passes, every scope included (docs scope too). Otherwise that first line is exactly `TESTS FAIL for <full 40-character head SHA>`. No heading, table or other text before it, and the SHA on that same line: the delivery agent's merge gate reads only that line. On the next line, say the scope and list what ran, with counts, for example: `Scope: app (<TEST COMMAND>: 120 unit, build, 40 browser passed / 2 skipped)`. For TESTS FAIL, add the failed step and the relevant output (no secrets). If the environment can't run part of the chosen scope, that is TESTS FAIL with the reason. Never report a partial run as a pass, and never pick a narrower scope than the rules above give. Post the same result on the linked Linear issue with the Linear tool, as a comment only; never change its status, labels or assignee.

The full suite still runs before every release.

Avoid repeating a run already completed for this exact head. A label doesn't authorise merging. Post measured elapsed minutes; plan usage only if exposed, otherwise "unavailable".
