Cursor test run for Maho1975/<REPO>: Composer 2.5, Fast off. No review, no code changes, no merge.

Accept only a `test: cursor` label-ADDED event on an open, non-draft, same-repository PR. Fetch its current head SHA. Post a start comment: role "test run", model, session ID, the Cursor run URL and the full head SHA.

Check out that exact SHA in this cloud workspace. Set up the test environment: <SETUP STEPS, e.g. throwaway database with fictional data>. Run `<INSTALL COMMAND>` then the full `<TEST COMMAND>`. Never use real data, secrets or private services, and never change files or push.

Post a PR comment starting **TESTS PASS** for `<full head SHA>` only when the full `<TEST COMMAND>` passes, otherwise **TESTS FAIL** for `<full head SHA>`, with the step that failed and the relevant output (no secrets). If the environment can't run part of the suite, that is TESTS FAIL with the reason. Never report a partial run as a pass. Post the same result on the linked Linear issue with the Linear tool, as a comment only; never change its status, labels or assignee.

Avoid repeating a run already completed for this exact head. A label doesn't authorise merging. Post measured elapsed minutes; plan usage only if exposed, otherwise "unavailable".
