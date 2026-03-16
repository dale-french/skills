---
name: write-a-skill
description: Creates new Claude Code skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, build, or improve a skill. Also triggers when discussing skill architecture, frontmatter options, or description optimization.
user-invocable: true
---

# Writing Skills

First, read the official authoring guide for the complete spec (frontmatter fields, string substitutions, invocation control, progressive disclosure, naming, and more): https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices

## Process

1. **Gather requirements** — ask user about:
   - What task/domain does the skill cover?
   - What specific use cases should it handle?
   - Does it need executable scripts or just instructions?
   - Any reference materials to include?

2. **Draft the skill** — create:
   - SKILL.md with concise instructions (under 500 lines)
   - Additional reference files if approaching that limit
   - Utility scripts if deterministic operations needed

3. **Review with user** — present draft and iterate:
   - Does this cover your use cases?
   - Anything missing or unclear?
   - Test with the models you plan to use (Haiku needs more guidance, Opus needs less)

## SKILL.md Template

```md
---
name: skill-name
description: Does X and Y for Z context. Use when user mentions A, B, or C, even if they don't explicitly ask for this skill.
argument-hint: "[required-arg] [optional-arg]"
---

# Skill Name

## Quick start

[Minimal working example]

## Workflows

[Step-by-step processes for complex tasks]

## Advanced features

See [reference.md](reference.md) for detailed API docs.
```

## Writing Descriptions

The description is the **only thing Claude sees** when deciding which skill to load. Claude tends to **undertrigger** skills — to not use them when they'd be useful. Combat this by making descriptions slightly "pushy": include adjacent keywords and contexts, not just exact matches.

**Good**: `Extracts text and tables from PDF files, fills forms, merges documents. Use when working with PDF files or when user mentions PDFs, forms, document extraction, or scanned documents.`

**Bad**: `Helps with documents.`

## Skill Archetypes

Three patterns emerge in practice — choose the one that fits:

- **Reference skills** — pure knowledge, short body. Brand guidelines, API conventions, style guides. Claude applies them inline alongside conversation context.
- **Workflow skills** — multi-phase processes with decision trees. Doc co-authoring, code review, deployment pipelines. Often use checklists for Claude to track progress.
- **Dispatcher skills** — thin SKILL.md that routes to bundled resources based on what the user needs. e.g. an internal-comms skill pointing to `examples/newsletter.md`, `examples/status-report.md`, etc.

## Review Checklist

After drafting, verify:

- [ ] Description includes "Use when..." triggers and is slightly pushy
- [ ] Description is in third person
- [ ] SKILL.md under 500 lines
- [ ] No time-sensitive info
- [ ] Consistent terminology throughout
- [ ] Concrete examples included
- [ ] References one level deep from SKILL.md
- [ ] Tested with intended model tier (Haiku/Sonnet/Opus)
- [ ] Naming uses lowercase-hyphenated form (prefer gerunds)
