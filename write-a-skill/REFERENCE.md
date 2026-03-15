# Write-a-Skill Reference

## Contents

- [Frontmatter fields](#frontmatter-fields)
- [Invocation control](#invocation-control)
- [String substitutions](#string-substitutions)
- [Progressive disclosure model](#progressive-disclosure-model)
- [Skill archetypes](#skill-archetypes)

## Frontmatter fields

All fields are optional. Only `description` is recommended.

| Field | Description |
|---|---|
| `name` | Display name and `/slash-command`. Lowercase letters, numbers, hyphens only. Max 64 chars. If omitted, uses directory name. |
| `description` | What the skill does and when to use it. Max 1024 chars. Claude uses this for auto-triggering. If omitted, uses first paragraph of body. |
| `argument-hint` | Hint shown during autocomplete. e.g. `[issue-number]` or `[filename] [format]` |
| `disable-model-invocation` | `true` = only the user can invoke via `/name`. Prevents Claude from auto-loading. Default: `false`. |
| `user-invocable` | `false` = hidden from `/` menu. Only Claude can invoke. Use for background knowledge. Default: `true`. |
| `allowed-tools` | Tools Claude can use without asking permission when skill is active. e.g. `Read, Grep, Glob` |
| `model` | Override which model runs the skill. |
| `context` | Set to `fork` to run in an isolated subagent. Skill content becomes the subagent's prompt (no conversation history). |
| `agent` | Which subagent type when `context: fork` is set. Options: `Explore`, `Plan`, `general-purpose`, or custom agents from `.claude/agents/`. |
| `hooks` | Hooks scoped to this skill's lifecycle. |

## Invocation control

The interaction between `disable-model-invocation` and `user-invocable` controls who can trigger the skill and how it loads:

| Frontmatter | User can invoke | Claude can invoke | Context loading |
|---|---|---|---|
| (defaults) | Yes | Yes | Description always in context; full skill loads when invoked |
| `disable-model-invocation: true` | Yes | No | Description NOT in context; full skill loads when user invokes |
| `user-invocable: false` | No | Yes | Description always in context; full skill loads when invoked |

**When to use each:**

- **Default** — most skills. Claude decides when it's relevant, user can also invoke directly.
- **`disable-model-invocation: true`** — skills with side effects or that need user-controlled timing: `/deploy`, `/commit`, `/send-slack-message`. You don't want Claude deciding to deploy.
- **`user-invocable: false`** — background knowledge that isn't actionable as a command. A `legacy-system-context` skill explains how an old system works; Claude should know this when relevant, but `/legacy-system-context` isn't a meaningful action.

## String substitutions

| Variable | Description |
|---|---|
| `$ARGUMENTS` | All arguments passed when invoking. If not present in content, appended as `ARGUMENTS: <value>`. |
| `$ARGUMENTS[N]` | Specific argument by 0-based index. e.g. `$ARGUMENTS[0]` for the first. |
| `$N` | Shorthand for `$ARGUMENTS[N]`. e.g. `$0`, `$1`. |
| `${CLAUDE_SESSION_ID}` | Current session ID. Useful for logging or session-specific files. |
| `${CLAUDE_SKILL_DIR}` | Directory containing the skill's SKILL.md. Use to reference bundled scripts regardless of working directory. |

**Example**:

```md
---
name: fix-issue
description: Fix a GitHub issue by number
disable-model-invocation: true
argument-hint: "[issue-number]"
---

Fix GitHub issue $ARGUMENTS following our coding standards.
```

`/fix-issue 123` → Claude receives "Fix GitHub issue 123 following our coding standards."

## Progressive disclosure model

Skills use a three-tier loading system to avoid wasting context tokens:

1. **Metadata** (~100 tokens) — name + description loaded at startup into system prompt for all installed skills
2. **SKILL.md body** (< 500 lines) — loaded only when the skill triggers
3. **Bundled resources** (unlimited) — loaded on demand when Claude follows a reference link; scripts execute without loading their source

This means you can bundle large reference docs, API specs, or example collections with zero context cost until Claude actually needs them.

**Budget**: skill descriptions consume ~2% of context window (fallback: 16,000 chars). Check with `/context`. Override with `SLASH_COMMAND_TOOL_CHAR_BUDGET` env var.

## Skill archetypes

Three patterns emerge in practice:

**Reference skills** — pure knowledge, short body. Brand guidelines, API conventions, style guides. Claude applies them inline alongside conversation context.

**Workflow skills** — multi-phase processes with decision trees. Doc co-authoring, code review, deployment pipelines. Often use checklists for Claude to track progress.

**Dispatcher skills** — thin SKILL.md that routes to bundled resources. An internal-comms skill that points to `examples/newsletter.md`, `examples/status-report.md`, etc. based on what the user needs.

Choose the archetype that fits your use case and structure accordingly.
