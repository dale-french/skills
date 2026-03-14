# Shortcut Reference

## Story templates

All sections are optional — omit any section that would be empty or just restate the title. Only include sections with real content.

### Feature

Description format:

```
[1-2 sentence "why" — only if not obvious from title/project context]

## Scope                                 (include when boundary isn't obvious from title)
- [What's in / out — e.g. "search results page only, not settings"]

## Acceptance criteria                   (include when definition of done is non-trivial)
- [Testable assertions — e.g. "Results render with dark theme tokens when user preference is dark"]

## Context                               (include when external refs exist)
- [Links to Figma, specs, Slack threads]

## Technical context                     (include when architecture is non-obvious)
- [File paths and functions, not prose — e.g. `src/search/ResultsList.tsx`, `useTheme` hook]

## Tracking events                       (include when analytics apply)
- [Analytics events to fire, AB test targeting]

## Design docs                           (always include this heading - i'll remove it if not needed)
- [Link to Figma design]
```

### Bug

Description format:

```
## Bug
[What is broken]

## Steps to reproduce
- [Step-by-step to trigger the bug]

## Expected vs Actual
- Expected: [what should happen]
- Actual: [what happens instead]

## Evidence                              (include when available)
- [Links to FullStory, logs, screenshots — link rather than paste inline]

---

## Technical context                     (include when fix is non-obvious)
```

### Chore / Tech Debt

Labels: `search:tech_debt`

Description format:

```
[1-2 sentence "why" — only if not obvious from title/project context]

## Scope                                 (include when boundary isn't obvious from title)
- [What's in / out]

## Acceptance criteria                   (include when definition of done is non-trivial)
- [Testable assertions — e.g. "No references to old config key remain in src/"]

## Technical context                     (include when architecture is non-obvious)
- [File paths, migration steps, dependencies]
```

### Experiment Cleanup (type: chore)

Labels: `search:cleanup`, `search:tech_debt`

Description format:

```
## Background
- Experiment: [experiment ID]
- Winning side: [variant name]
- Related story: [link to original experiment story]
- Original PRs: [links if found]

## Acceptance criteria
- Winning variant kept as default behavior
- Toggles and unused code removed
- Tests updated / created to reflect permanent state
- Translations cleaned up

## Files to modify
- [auto-populated from codebase grep]
```

## Bug custom fields

When creating bug stories, always set these five custom fields: **Severity**, **Priority**, **Bug Type**, **Source of Discovery**, **Regression**.

### How to set them

1. Call `custom-fields-list` to get current field IDs and value IDs
2. Match field/value names from the guidelines below to the IDs returned
3. Call `stories-update` with the `custom_fields` array after creating the story

### Judgement guidelines

**Severity** — impact on users/business ("how bad is this?")
- **Critical**: loss of bookings or crashes the app under common flows
- **High**: could cause loss of bookings or crashes under less-common flows
- **Medium**: unexpected/undesirable behavior, but doesn't disrupt core function
- **Low**: no noticeable application breakdown

**Priority** — urgency ("how soon must we fix it?"). Can differ from severity — e.g. high priority + low severity when blocking another team, or high severity + low priority for a critical bug behind a disabled toggle.
- **High**: must go into the current sprint
- **Medium**: should be prioritized for upcoming sprints
- **Low**: can be fixed at a later date

**Bug Type** — match to the affected area:
- **User Interface (UI)**: layout, design, usability
- **Performance**: slow response, memory leaks
- **Security**: vulnerabilities, unauthorized access
- **Functional**: incorrect behavior, wrong results
- **Integration**: issues with external systems/providers
- **Tracking**: analytics, attribution, event tracking

**Source of Discovery** — infer from context:
- **User Reported**: reported by user or support channel (default if unclear)
- **Manual QA**: found during manual testing
- **Automated Tests**: caught by CI/tests
- **Monitoring / Alert**: flagged by monitoring or alerts

**Regression** — was this working before?
- **Yes**: introduced by a recent change
- **No**: longstanding issue
- **Unknown**: not sure (default if unclear)

## MCP tools cheat sheet

### Stories

- `stories-create` - name (required), description, story_type (feature/bug/chore), group_id, workflow_state_id, story_template_id, labels
- `stories-get-by-id` - story_id (number)
- `stories-search` - query, group_id
- `stories-update` - story_id + fields to update
- `stories-create-comment` - story_id, text
- `stories-add-task` - story_id, description
- `stories-assign-current-user` / `stories-unassign-current-user` - story_id

### Epics

- `epics-create` - name (required), description
- `epics-get-by-id` - epic_id
- `epics-search` - query

### Other

- `custom-fields-list` - no params, returns field IDs and value IDs (use for bug custom fields)
- `workflows-get-default` - no params, returns workflow states
- `teams-list` - no params
- `labels-list` - no params
- `labels-create` - name, color

## Formatting rules

- No checkboxes (`- [ ]`) in descriptions - use plain bullets (`-`)
- Story link format: `https://app.shortcut.com/vio/story/{story_id}`
- Epic link format: `https://app.shortcut.com/vio/epic/{epic_id}`
