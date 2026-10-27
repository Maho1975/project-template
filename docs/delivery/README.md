# Delivery

Owner: Claude (Opus). Work board: Linear, project "<LINEAR PROJECT>". The rules are in [AGENTS.md](../../AGENTS.md); the flow is drawn in [workflow.md](workflow.md). This folder also holds the two Cursor automation prompts.

## Cursor automations (Matt's account)

- **<PROJECT> - Cursor Tests**: trigger PR label `test: cursor` added; Composer 2.5, Fast off; terminal allowed; Linear MCP on; prompt: [cursor-tests-prompt.md](cursor-tests-prompt.md). Tests only, every PR.
- **<PROJECT> - Grok Reviewer**: trigger PR label `review: grok` added; Grok 4.7 High, Fast off; terminal allowed; no PR creation, approval or code-changing tools; Linear MCP on; prompt: [cursor-reviewer-prompt.md](cursor-reviewer-prompt.md).
