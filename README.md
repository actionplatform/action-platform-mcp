# Action Platform — MCP plugin

Skills, plugin manifests and marketplace entries that put the [Action Platform](https://github.com/actionplatform/action-platform) MCP server inside AI coding clients: Claude Code, Codex, Cursor and anything else that speaks MCP.

The server itself lives in the main repository and installs from PyPI:

```bash
pip install "action-platform[mcp]"
action-platform mcp                 # stdio
action-platform mcp --http          # http://127.0.0.1:8765/mcp
```

## Claude Code

```bash
/plugin marketplace add actionplatform/action-platform-mcp
/plugin install action-platform@action-platform
```

## Codex

`.codex-plugin/plugin.json` and `.agents/plugins/marketplace.json` describe the same plugin for Codex; point the client at this repository.

## Any client

Add to `.mcp.json`:

```json
{ "mcpServers": { "action-platform": { "command": "uvx", "args": ["--from", "action-platform[mcp]", "action-platform-mcp"] } } }
```

## Layout

| Path | What |
|---|---|
| `skills/` | 13 skills: preview first, ask before anything leaves the machine (`scaffold-project`, `install-platform`, `push-project`, `start-branch`, `open-pull-request`, `release`, `deploy`, `rollback`, `diagnose`, `add-cloud`, `add-service`, `edit-configuration`, `fix-gitflow`) |
| `.claude-plugin/` | Claude Code plugin and marketplace manifests |
| `.codex-plugin/`, `.agents/plugins/` | Codex plugin and marketplace manifests |
| `.mcp.json` | the server entry the plugins reference |
| `.app.json` | app catalog metadata |

Tools, prompts, local vs remote mode and permissions: [docs/use_mcp.md](https://github.com/actionplatform/action-platform/blob/master/docs/use_mcp.md) in the main repository.
