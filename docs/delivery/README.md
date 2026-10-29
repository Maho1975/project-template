# Delivery

Owner: Claude (Opus). Work board: Linear, project "<LINEAR PROJECT>". The rules are in [AGENTS.md](../../AGENTS.md); the flow is drawn in [workflow.md](workflow.md). This folder also holds the two Cursor automation prompts and an example entry for the delivery agent's projects config ([projects.example.json](projects.example.json)).

## Cursor automations (owner's account)

- **<PROJECT> - Cursor Tests**: trigger PR label `test: cursor` added; Composer, Fast off; terminal allowed; Linear MCP on; prompt: [cursor-tests-prompt.md](cursor-tests-prompt.md). Tests only, every PR, scoped to the change. This is the only test run on a PR.
- **<PROJECT> - Grok Reviewer**: trigger PR label `review: grok` added; Grok at High effort, Fast off; review only, no test run; terminal for at most one narrow command to prove a finding; no PR creation, approval or code-changing tools; Linear MCP on; prompt: [cursor-reviewer-prompt.md](cursor-reviewer-prompt.md).

Set the model and effort in each automation to match the builder pool below. The prompts name the model only as the value the automation runs; the verdict reports what actually ran.

## Delivery agent

One delivery agent on the owner's PC serves every project. Each project is one entry in its owner-managed projects config, a private file outside any repo (owner-only, mode 0600), with its own state directory. Nothing project-specific is built into the agent: repository, Linear team and project, issue prefix, base branches, labels, builders and thresholds all come from the entry. Copy [projects.example.json](projects.example.json), fill the placeholders and add it under `projects`.

### Builder pool

`builders` lists every model the project may use. Each has a `model` id, a display `name`, a `family`, a `lane` and an `active` flag. At most one builder per lane is active, and that builder sets the model the lane runs. Swapping a model, bringing an old one back or adding a new one is a config change only; no code or prompt changes.

`repair_preference` lists every model once, in the order to try for repairs. `repair_rounds` (default 2) caps repair rounds per card. `stall_minutes` (default 30) is the idle limit for PRs and waiting repairs.

### What the agent does

- **Dispatch.** Takes Todo cards with a lane's ready label by priority, then age. A card goes to the lane its ready label names. Cards run in parallel only when their Scope paths don't overlap.
- **Publish.** Opens one PR per card linking it, adds `test: cursor` and the review label for the other family, and moves the card's "With" label as work changes hands.
- **Merge.** Squash-merges on the one merge rule, SHA pinned, deletes the branch, moves the card to In Review + Verify. A merged repair also moves its original card to Verify.
- **Repairs.** On a current-head Changes required or TESTS FAIL it creates one repair card per failing head, titled `Repair for <original>: <PR title>`, for the first active builder in `repair_preference` that has not written code on that PR chain, another family first. The card carries the failing receipts and the lane's ready label, and is unassigned. If that lane can't start the session itself, the agent comments once on the failing PR so the delivery owner starts it. After `repair_rounds`, or when no builder is eligible, the original card is Blocked and `needs_you` is raised.
- **Stall check.** Every open PR with `test: cursor` or a review label is watched. No Cursor run five minutes after a head lands, or a run in progress for over 45 minutes, gets one relabel of both labels per SHA; if nothing runs fifteen minutes later it posts one card note. A PR with no new commit, receipt, repair or running check for `stall_minutes` raises `needs_you` naming the PR and the reason.
- **Dead sessions.** A build with no new activity for 20 minutes is probed. A confirmed dead session is archived with its checkout and the card is continued once in a new session.
- **Capacity.** A provider capacity error releases the card to Todo and retries on the same builder after 5, 15 and 30 minutes, then blocks it with a plain reason.
- **Builds view.** It writes a private snapshot (now building, queue, review, history, `needs_you`, builders) that a dashboard can read. Every name in it comes from the running model and effort, never a lane name, unless the model is unknown.

Restart the agent only at idle (`delivery.py --wait-idle`), so no build is cut off. Release scripts use that command, not a process search.

### Trusted receipts

The merge gate reads only trusted integration identities (`trusted_github`, `trusted_linear`), never the builder or the agent itself. A test receipt's first line is exactly `TESTS PASS for <40-character SHA>` or `TESTS FAIL for <40-character SHA>`. A review verdict is a GitHub review on that commit whose first line is `PASS for <SHA>` or `Changes required for <SHA>`. An ordinary comment can carry a test result but never approves a PR.
