---
name: write-a-skill
description: Use when creating or editing a skill, or discussing skill structure, frontmatter, or descriptions.
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
description: Use when A, B, or C.
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

The description is written for the model, not for a human reader. It stays in context every turn, and it's the only thing Claude sees when deciding whether to invoke the skill. Its job is to say *when* to invoke the skill. Claude learns what the skill does from the body after it loads.

- **Lead with the trigger.** Start with "Use when…" or "Use whenever…". Add a short what-it-is phrase only if the name doesn't already make that clear.
- **One trigger per distinct case.** List each situation the skill handles. Don't pad with synonyms for the same case: they cost context on every turn and don't trigger more reliably.
- **No "I can…" or "You can use this…" phrasing.** Address the invoking model directly with "Use when…".

**Good**: `Use whenever leaving comments on GitHub.`

**Good**: `Writing documents for agents. Use when creating or editing skills, or modifying AGENTS.md or CLAUDE.md.`

**Bad**: `Helps with documents.` (no trigger)

**Bad**: `A handy tool you can use to write great PR comments!` (written for a human, no trigger)

If the skill sets `disable-model-invocation: true`, the model never sees the description. Then write it for the human: a one-line summary, no trigger list.

## Skill Archetypes

Three patterns emerge in practice — choose the one that fits:

- **Reference skills** — pure knowledge, short body. Brand guidelines, API conventions, style guides. Claude applies them inline alongside conversation context.
- **Workflow skills** — multi-phase processes with decision trees. Doc co-authoring, code review, deployment pipelines. Often use checklists for Claude to track progress.
- **Dispatcher skills** — thin SKILL.md that routes to bundled resources based on what the user needs. e.g. an internal-comms skill pointing to `examples/newsletter.md`, `examples/status-report.md`, etc.

## Review Checklist

After drafting, verify:

- [ ] Description is model-facing: leads with "Use when…" and covers each distinct trigger once, with no synonym padding
- [ ] SKILL.md under 500 lines
- [ ] No time-sensitive info
- [ ] Consistent terminology throughout
- [ ] Concrete examples included
- [ ] References one level deep from SKILL.md
- [ ] Tested with intended model tier (Haiku/Sonnet/Opus)
- [ ] Naming uses lowercase-hyphenated form (prefer gerunds)
