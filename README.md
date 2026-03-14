# Agent Skills

A collection of reusable [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills.

## Installation

Clone this repo as your personal skills directory:

```bash
git clone https://github.com/dale-french/skills ~/.claude/skills
```

Claude Code automatically discovers `SKILL.md` files in `~/.claude/skills/` subdirectories, so all skills become available immediately across every project — no restart needed.

## Skills

### Project Tracking

- _shortcut_ — Manage [Shortcut](https://www.shortcut.com/) stories and epics via natural language.

### Writing Skills

- _write-a-skill_ — Create new skills with proper structure, progressive disclosure, and bundled resources.

## Configuration

### Shortcut

The shortcut skill requires the [Shortcut MCP server](https://github.com/useshortcut/mcp-server-shortcut/blob/main/docs/local-server.md#claude-code) to be configured in Claude Code.

Once the MCP server is set up, edit `shortcut/CONFIG.md` with your team and workflow IDs:

```
team_id: <your-team-id> (<Team Name>)
workflow_id: <your-workflow-id> (<Workflow Name>)
```

To find these values, invoke the skill and it will auto-detect your team. Or use the Shortcut MCP tools directly:

- `teams-list` — lists available teams and their IDs
- `workflows-list` — lists workflows and their IDs
