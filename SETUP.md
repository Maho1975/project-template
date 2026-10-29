# New project setup

Do these once, in order, then delete this file.

1. Replace every `<PLACEHOLDER>` in AGENTS.md, docs/ and .github/ (search for `<`). In docs/delivery/cursor-tests-prompt.md, fill in the path lists for each test scope and the test commands.
2. In Linear, create the project with states Backlog, Todo, In Progress, In Review, Done.
3. In Linear, add labels `sol-ready`, `opus-ready`, `sol-test`, `sol-base:main`, `Verify`, `Review`, `Blocked`, `End of card`, plus the five "With" labels named in docs/delivery/projects.example.json.
4. In Linear, add a document called "Delivery scorecard".
5. In GitHub, add labels `test: cursor`, `review: grok`, `review: claude-opus`, `tests: full`, `do not merge`, `blocked`.
6. In GitHub repo settings, turn on "Automatically delete head branches".
7. In Cursor, create "<PROJECT> - Cursor Tests" from docs/delivery/README.md.
8. In Cursor, create "<PROJECT> - Grok Reviewer" from docs/delivery/README.md.
9. Add this project to the delivery agent's projects config on your PC: copy docs/delivery/projects.example.json into it, fill the placeholders, and list the builders this project uses.
10. Restart the delivery agent at idle (`delivery.py --wait-idle`, then restart the service).
11. Run one disposable `sol-test` card, then one real low-risk card end to end.
