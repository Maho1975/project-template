# Builds page (the first card)

The Builds page is the first thing every project builds, before any product card. It is where the owner watches the delivery loop: what is building, with which model, what is waiting, and whether anything needs them. Until it runs, nobody can see the loop without reading Linear, GitHub and logs.

The rule in [AGENTS.md](../../AGENTS.md) is short: no product card goes to Todo until the Builds page cards are Done. This page says what those cards build.

## What it follows

The page follows the project's model routing file and process map (`docs/delivery/model-routing.md`; until the project has one, the builder pool in the delivery agent's config). The process map's steps are the page's steps, and the routing file's lists and tasks are what the page uses to say who builds, tests and reviews each card. When the routing file changes, the page changes with it, with no code change.

## What it shows

**Now**

- A one-line headline: whether the loop is running, paused (and why, such as the review limit), or needs the owner.
- The routing list in use, and for each provider in it, quota against its soft limit, read from the agent's latest reading.
- The process map, from the owner's Start gate through build, checks, repair and merge to the owner's Finish gate, with the cards at each step and the loop counters (repair rounds, relabels, stalls).
- Each card building now: title, the model and effort actually running, its tier, the planned tester and reviewer, time in step, and a note when a card kept its model after a list switch.
- Checks per open PR: the test receipt and review verdict for the exact head, with the model that gave each.
- Up next: the Ready queue in dispatch order, each with the model it will get, resolved from the list in use.
- `needs_you`: anything waiting on the owner, with the one thing to do.

**Routing**

- Every routing list, which one is in use, and why (main, or a provider out).
- Which providers are out, since when, and when they are expected back.
- The models roster from config: name, family, tier, lane, active.
- The task table for the list in use, with cells that differ from the main list marked.

**History**

- Merged and accepted cards with builder, reviewer, repair rounds and token cost, matching the scorecard.

## Fully dynamic

- Every model name, effort, tier, lane, label, step and threshold comes from config (the routing file and the agent's projects config) or from live data (the agent's Builds snapshot, receipts, quota readings). Nothing is hard-coded: not a model name, not a lane list, not a checker name.
- A label names the model and effort that actually ran. It falls back to the neutral lane name only when the model is unknown.
- Adding, swapping or retiring a model, changing a list, or adding a step to the process map shows on the page with no code change. The acceptance test proves this.
- The page reads only. It never starts, stops, merges or relabels anything.
- It refreshes on its own, and says how old its data is when the snapshot is stale.

## Design

The visual style comes from the project's own approved design reference (`<DESIGN REFERENCE>` in AGENTS.md). Do not copy another project's Builds page design. When the project has no approved design yet, the first card is the design: a Builds page mockup in a proposed style, which the owner approves before the build card goes to Todo.

## The first cards

Write these when the project is set up (SETUP.md). They follow every delivery rule: acceptance test and screen width on each, top tier first for design and mockup work, a reviewer that is a different model from the builder.

1. **Builds page mockup.** Builds the Now, Routing and History views in the project's approved style, with sample data shaped like the real snapshot. The owner approves it. Skip the mockup only if the owner says the approved design already covers the page.
2. **Builds page data.** The page's server side reads the routing file and the agent's Builds snapshot into one view model. No model, lane or checker name in code. Acceptance test: add a model to config and change the list in use in a fixture; the view model shows both with no code change.
3. **Builds page screens.** Builds the views from the approved mockup on that view model. Acceptance test: side-by-side screenshots against the mockup at the card's screen widths, plus a fixture with an unknown model that shows the neutral lane name.

Split further if a card would pass roughly 25 files. Product cards start after these are Done.
