# Delivery workflow

The rules are in [AGENTS.md](../../AGENTS.md). This page draws them. Which model runs each lane is set in the delivery agent's config, not here.

## 0. First cards

```mermaid
flowchart LR
    A["Project set up<br/>SETUP.md done, sol-test card passed"] --> B["Builds page cards<br/>Mockup in the project's design, data, screens<br/>docs/delivery/builds-page.md"]
    B --> C["Owner accepts the Builds page"]
    C --> D["Product cards may go to Todo"]
```

## 1. Card to pull request

```mermaid
flowchart TD
    A["Delivery owner writes the card<br/>Outcome, acceptance, risk, boundaries<br/>Owner approves it"] --> B{"Size check<br/>One session, about 25 files"}
    B -- "Too big" --> A2["Split into smaller cards"]
    A2 --> B
    B -- "Fits" --> C["Todo (Ready)<br/>Builder, reviewer and Scope paths named"]
    C --> D1["Codex lane<br/>Core logic, data, imports"]
    C --> D2["Claude lane<br/>Low-risk builds, screens, refactors"]
    D1 --> E["Build starts<br/>In Progress, branch from main<br/>Parallel only if Scope paths don't overlap"]
    D2 --> E
    E --> F["PR opened<br/>Linked on the card<br/>test: cursor + the review label of a different model"]
```

## 2. Tests, review and repairs

```mermaid
flowchart TD
    A["PR open, In Review"] --> B["Cursor tests<br/>test: cursor, scoped to the change"]
    A --> C["Independent review<br/>A different model from the builder, fresh session<br/>Review only, no test run"]
    B --> D{"Both pass on the same SHA?<br/>Nothing contradicts"}
    C --> D
    D -- "Fail" --> E["Repair card for a different builder<br/>One that hasn't written code on this PR"]
    E -- "Push, then relabel both labels" --> A
    E -- "After 2 rounds" --> F["Cancel the card<br/>Top tier rewrites and rebuilds<br/>No eligible builder: Blocked + needs_you"]
    D -- "Pass" --> G["Ready to merge<br/>Go to step 3"]
    S["Stall check<br/>No run 5 min after a push: relabel once<br/>Idle 30 min: needs_you"]
    P["Platform out<br/>Checkpoint on card, next active builder<br/>Pause, never skip review"]
```

## 3. Merge to Done

```mermaid
flowchart TD
    A["Delivery agent merge check<br/>TESTS PASS + PASS on exact head<br/>No do not merge label"] --> B["Mirror verdicts to PR<br/>Linear-only verdicts copied over"]
    B --> C["Squash merge<br/>SHA pinned, branch deleted"]
    C --> D["Card to Verify<br/>Merged is not deployed"]
    D --> E["Owner checks running result<br/>Accepts: Verify to Done"]
    E --> F["End of card<br/>Clean sessions, worktrees, test DBs<br/>Scorecard row with model and token cost, one note to the owner"]
```

## Linear states

Backlog → Todo (Ready) → In Progress → In Review → In Review + `Verify` → Done
