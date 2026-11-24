# saymock for Claude Code

[![Saymock MCP connector](https://glama.ai/mcp/connectors/com.saymock/saymock/badges/score.svg)](https://glama.ai/mcp/connectors/com.saymock/saymock)

[saymock](https://saymock.com/en/?utm_source=claude-plugin-readme&utm_medium=github) is a collaborative design board your own agent can draw on: screens
are HTML + Tailwind, your team comments and reviews them live, and Claude turns them into code.

This plugin connects Claude Code to saymock and adds skills for the common jobs:

| Skill              | When                                                                    |
| ------------------ | ----------------------------------------------------------------------- |
| `saymock-design`   | Design new screens or a flow in saymock (method, components, templates) |
| `design-from-code` | Recreate the app's existing screens in saymock from the codebase        |
| `implement-design` | Turn saymock screens into code that follows the project's conventions   |
| `design-review`    | Review screens for UX, consistency and accessibility                    |

The MCP server (`https://app.saymock.com/mcp`) brings the tools (`create_frame`,
`get_screenshot`, `export_code`…) and the `inbox` prompt: comments your team addresses to the
agent on the board.

## Install

```bash
claude plugin marketplace add saymock/claude-plugin
claude plugin install saymock@saymock
```

Or in Claude Code: `/plugin marketplace add saymock/claude-plugin`, then `/plugin install
saymock@saymock`.

Then `/mcp` → saymock → sign in: a browser page asks which workspace Claude may use. You need a
saymock account (free trial at [app.saymock.com](https://app.saymock.com)).

If you added the server by hand before (`claude mcp add … saymock …`), remove it
(`claude mcp remove saymock`): otherwise the tools show up twice.

Self-hosted server or local development: the plugin connects to `https://app.saymock.com/mcp`;
add your own server by hand instead (`claude mcp add --transport http saymock http://localhost:4040/mcp`).

## Privacy

The plugin only connects Claude Code to your saymock account; what the agent reads and writes
stays in the workspace you authorize. See the [privacy policy](https://saymock.com/en/privacy/).

## License

MIT. This repository is published from saymock's main repository: open issues here, changes are
made there.
