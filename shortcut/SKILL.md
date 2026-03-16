---
name: shortcut
description: Create, view, and search Shortcut stories and epics from natural language. Use when user invokes /shortcut to manage project tracking.
disable-model-invocation: true
user-invocable: true
argument-hint: "[natural language request]"
---

# Shortcut

Manage Shortcut stories, epics, and searches from natural language. Read [REFERENCE.md](REFERENCE.md) for templates and MCP tool details.

## Setup

Before routing, check if Shortcut MCP tools are available (e.g. `stories-create`). If not, link the user to the setup guide and stop:

https://github.com/useshortcut/mcp-server-shortcut/blob/main/docs/local-server.md#claude-code

## Quick start

```
/shortcut create a feature for adding dark mode to search results
/shortcut file a bug for search crashing on mobile Safari
/shortcut cleanup experiment search-widget-v2
/shortcut show story 12345
/shortcut find stories about price freeze
/shortcut move story 12345 to In Progress
```

## Intent routing

Parse `$ARGUMENTS` and route to the matching action:

| Intent       | Signal words                             | Action                                                                                |
| ------------ | ---------------------------------------- | ------------------------------------------------------------------------------------- |
| Create story | "create", "add", "new", "file", "report" | Infer type, apply template, `stories-create`. For bugs: set custom fields (see below) |
| Create epic  | "epic"                                   | `epics-create`                                                                        |
| View         | story ID, Shortcut URL, "show", "get"    | `stories-get-by-id`                                                                   |
| Search       | "find", "search", "my stories", "list"   | `stories-search`                                                                      |
| Update       | "update", "set", "move", "change", "edit" | `stories-update` (by ID or search first)                                              |

## Type inference

When type is not explicit, infer from keywords:

- **Bug**: crash, broken, fix, error, regression, not working, fails, 500
- **Chore / Tech Debt**: cleanup, refactor, update deps, tech debt, migrate, remove, delete — always label `search:tech_debt`
- **Feature**: default when no other signals match

If ambiguous, confirm with the user before creating.

## Cleanup stories

When the request mentions "cleanup" + an experiment/toggle ID:

1. Grep the codebase for the experiment ID to find affected files
2. Search Shortcut for related stories about that experiment
3. Apply the cleanup template from REFERENCE.md, auto-populating "Files to modify"
4. Add labels: `search:cleanup`, `search:tech_debt`, `AI`

## Bug custom fields

When creating a **bug** story, always set the five custom fields. Use your best judgement to pick values based on the bug description. See REFERENCE.md § "Bug custom fields" for judgement guidelines.

Fields: **Severity**, **Priority**, **Bug Type**, **Source of Discovery**, **Regression**

Flow:

1. Call `custom-fields-list` to get current field/value IDs
2. Create the bug story via `stories-create`
3. Call `stories-update` with the `custom_fields` array to set all five fields
4. If unsure about a value, prefer the moderate/default option (Medium severity, Medium priority, Functional type, User Reported source, Unknown regression)

## Defaults

Read [CONFIG.md](CONFIG.md) for `team_id` and `workflow_id`. If CONFIG.md is missing `team_id`:

1. Call `teams-list` to show available teams
2. If only one team, auto-select it. If multiple, ask the user to pick.
3. Save to CONFIG.md

Use `team_id` for `group_id` param (never `mention_name`). Always output a clickable link after creation (see link format in REFERENCE.md).

## Writing style

- **Title is the task.** State what needs to happen directly. Verb-first, scannable in a list. e.g "Add dark mode to search results page"
- **Lead with why.** The first line of the description answers "why now?" or "why this matters." If the title makes the why obvious, skip it entirely.
- **Description is context, not a spec.** Only include sections that carry information the assignee doesn't already have. Omit any template section that would restate the title or contain placeholder text. Always start minimal — add sections only when they carry information the assignee doesn't already have.
- **Quote, don't summarize.** When the user provides user feedback or error messages, quote them directly rather than paraphrasing.
- **Criteria as assertions.** Write acceptance criteria as testable statements, not vague outcomes. "Search results page uses dark theme tokens when user preference is dark" not "dark mode works."
- **Point to code, don't describe it.** In Technical context, prefer file paths (`src/search/api.ts`) over prose descriptions of architecture. Readers can click; agents can grep.
- **Write for two readers.** Stories are read by teammates and by agents building the feature. Prefer explicit scope, concrete file paths, and testable criteria over implied context.

## Rules

- No checkboxes in descriptions - use plain bullets only
- If MCP tools are unavailable, show the Setup steps above and stop
- Keep story titles concise (under 80 chars)
- When creating stories, show the user a preview of title, type, and description before calling the API. For bugs, also preview the inferred custom field values (severity, priority, bug type, source, regression).
