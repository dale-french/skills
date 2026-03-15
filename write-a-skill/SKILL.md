---
name: write-a-skill
description: Creates new Claude Code skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, build, or improve a skill. Also triggers when discussing skill architecture, frontmatter options, or description optimization.
user-invocable: true
---

# Writing Skills

Before drafting, read [REFERENCE.md](REFERENCE.md) for the complete frontmatter reference, invocation control table, and string substitutions. For the latest official guidance, see https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices

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

## Skill Structure

```
skill-name/
├── SKILL.md           # Main instructions (required)
├── reference.md       # Detailed docs (loaded on demand)
├── examples.md        # Usage examples (loaded on demand)
└── scripts/           # Utility scripts (executed, not loaded)
    └── helper.py
```

Keep references one level deep from SKILL.md — avoid chains where reference A links to reference B links to reference C. For reference files over 100 lines, add a table of contents at the top.

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

See [REFERENCE.md](REFERENCE.md) § "Frontmatter fields" for all available options including `disable-model-invocation`, `user-invocable`, `allowed-tools`, `context`, `agent`, and `model`.

## Naming Conventions

- Lowercase letters, numbers, and hyphens only (max 64 chars)
- Cannot contain "anthropic" or "claude"
- Prefer gerund form: `processing-pdfs`, `managing-databases`, `testing-code`
- Avoid vague names: `helper`, `utils`, `tools`

## Writing Descriptions

The description is the **only thing Claude sees** when deciding which skill to load. It's surfaced in the system prompt alongside all other installed skills.

Claude tends to **undertrigger** skills — to not use them when they'd be useful. Combat this by making descriptions slightly "pushy": include adjacent keywords and contexts, not just exact matches.

**Format**:

- Max 1024 chars, write in third person
- First sentence: what it does
- Second sentence: "Use when [specific triggers]"
- Include both obvious and non-obvious trigger contexts

**Good example**:

```
Extracts text and tables from PDF files, fills forms, merges documents. Use when working with PDF files or when user mentions PDFs, forms, document extraction, or scanned documents.
```

**Bad example**:

```
Helps with documents.
```

## Degrees of Freedom

Match instruction specificity to how fragile the task is:

- **High freedom** (text guidelines): multiple valid approaches, context-dependent decisions — e.g. code review criteria
- **Medium freedom** (templates with parameters): a preferred pattern exists but variation is acceptable — e.g. report generation
- **Low freedom** (exact scripts): operations are fragile, consistency is critical — e.g. database migrations, deployments

## Arguments and Dynamic Context

Skills support string substitutions — see [REFERENCE.md](REFERENCE.md) § "String substitutions" for `$ARGUMENTS`, `$0`/`$1`, `${CLAUDE_SKILL_DIR}`, and `${CLAUDE_SESSION_ID}`.

For injecting live data (git diffs, API responses) before Claude sees the prompt, use the `` !`command` `` syntax:

```md
## Current changes
- Diff: !`git diff --staged`
```

The command runs first and its output replaces the placeholder.

## When to Add Scripts

Add utility scripts when:

- Operation is deterministic (validation, formatting)
- Same code would be generated repeatedly across invocations
- Errors need explicit handling (scripts should solve, not punt to Claude)

Scripts save tokens and improve reliability vs generated code.

## When to Split Files

Split into separate files when:

- SKILL.md approaches 500 lines
- Content has distinct domains (finance vs sales schemas)
- Advanced features are rarely needed (progressive disclosure)

## Review Checklist

After drafting, verify:

- Description includes triggers ("Use when...") and is slightly pushy
- Description is in third person
- SKILL.md under 500 lines
- No time-sensitive info
- Consistent terminology throughout
- Concrete examples included
- References one level deep from SKILL.md
- Tested with intended model tier (Haiku/Sonnet/Opus)
- Naming uses lowercase-hyphenated gerund form
