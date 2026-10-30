# Decisions

The record for product choices the owner has accepted. A proposal is not approved until the owner answers it.

## Chosen on <DATE>

| Topic | Decision |
| --- | --- |
| Repository | Private GitHub repository <OWNER>/<REPO>. |

## Still open

- <Open question, owner, what it blocks>

## Four additions to the delivery rules — 30 October 2026

- Rule 1: a card's target screen widths include the owner's real widths and display scaling, and feed the screenshot run.
- Rule 3: a setup failure (tool, database version, fixture, design reference) is not a repair round.
- Rule 4: a handoff counts only once the work has started. A run with no verdict or a dropped builder session is retried once on the same head, then shown as needing attention.
- Rule 12: scorecard rows also record time spent waiting between steps.
- New rule 13: a claimed card is frozen; new scope goes on a new card, while Critical and High defects in the claimed work are repaired on its PR.
