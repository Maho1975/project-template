# Delivery workflow

The rules are in [AGENTS.md](../../AGENTS.md). This page draws them.

## 1. Card to pull request

```mermaid
flowchart TD
    A["Matt writes the card<br/>Outcome, acceptance, risk, boundaries"] --> B{"Size check<br/>About 25 files, under 15 min"}
    B -- "Too big" --> A2["Split into smaller cards"]
    A2 --> B
    B -- "Fits" --> C["Todo (Ready)<br/>Builder and reviewer named"]
    C --> D1["Sol 6.1<br/>Core logic, imports"]
    C --> D2["Claude Sonnet<br/>Low-risk builds"]
    C --> D3["Claude Opus<br/>Wide refactors"]
    C --> D4["GPT-6 Luna<br/>Mechanical fixes"]
    D1 --> E["Build starts<br/>In Progress, branch from main"]
    D2 --> E
    D3 --> E
    D4 --> E
    E --> F["PR opened<br/>Linked on the card"]
```

## 2. Tests, review and repairs

```mermaid
flowchart TD
    A["PR open, In Review"] --> B["Cursor tests<br/>test: cursor, Composer 2.5"]
    A --> C["Independent review<br/>Codex-built → Claude Opus<br/>Claude-built → Grok 4.7 High"]
    B --> D{"Both pass on the same SHA?<br/>Nothing contradicts"}
    C --> D
    D -- "Fail" --> E["Same builder fixes<br/>Back to In Progress"]
    E -- "Re-runs tests and review" --> A
    E -- "After 2 repair rounds" --> F["Blocked<br/>Astra diagnoses"]
    D -- "Pass" --> G["Ready to merge<br/>Go to step 3"]
    P["Platform out<br/>Checkpoint on card, next lane<br/>Pause, never skip review"]
```

## 3. Merge to Done

```mermaid
flowchart TD
    A["Delivery agent merge check<br/>TESTS PASS + PASS on exact head<br/>No do not merge label"] --> B["Mirror verdicts to PR<br/>Linear-only verdicts copied over"]
    B --> C["Squash merge<br/>SHA pinned, branch deleted"]
    C --> D["Card to Verify<br/>Merged is not deployed"]
    D --> E["Matt checks running result<br/>Accepts: Verify to Done"]
    E --> F["End of card<br/>Clean sessions, worktrees, test DBs<br/>Scorecard row, one note to Matt"]
```

## Linear states

Backlog → Todo (Ready) → In Progress → In Review → In Review + `Verify` → Done
