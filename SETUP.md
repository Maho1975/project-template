# New project setup

Do these once, in order, then delete this file.

1. Replace every `<PLACEHOLDER>` in AGENTS.md, docs/ and .github/ (search for `<`).
2. In Linear, create the project with states Backlog, Todo, In Progress, In Review, Done.
3. In Linear, add labels `sol-ready`, `sol-base:main`, `Verify`, `Review`, `Blocked`, `End of card`.
4. In Linear, add a document called "Delivery scorecard".
5. In GitHub, add labels `test: cursor`, `review: grok`, `review: claude-opus`, `do not merge`, `blocked`.
6. In GitHub repo settings, turn on "Automatically delete head branches".
7. In Cursor, create "<PROJECT> - Cursor Tests" from docs/delivery/README.md.
8. In Cursor, create "<PROJECT> - Grok Reviewer" from docs/delivery/README.md.
9. Point the delivery agent on your PC at this repo and Linear project.
10. Run one disposable `sol-test` card, then one real low-risk card end to end.
