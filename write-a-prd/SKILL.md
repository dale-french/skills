---
name: write-a-prd
description: Create a Product Requirements Document through user interview, codebase exploration, and technical design. Use when user wants to write a PRD, create a product requirements document, plan a new feature, spec out a project, or draft requirements for an initiative.
argument-hint: "[optional: brief description of the feature or problem]"
user-invocable: true
---

# Write a PRD

Create a PRD through structured discovery, then output it wherever the user wants.

You may skip steps if context makes them unnecessary.

## Step 1: Understand the problem

Ask the user for a detailed description of:
- The problem they want to solve
- Any ideas they have for the solution
- Who this is for

If arguments were provided, use them as the starting point and ask clarifying questions about gaps.

## Step 2: Explore the codebase

Examine the repo to verify assertions and understand the current state. Skip if the PRD is not code-related.

## Step 3: Interview

Invoke the `/poke-holes` skill against the plan so far. This will stress-test the design and resolve open questions.

## Step 4: Design

Sketch the major modules to build or modify. Look for opportunities to extract deep modules — components that encapsulate significant functionality behind a simple, stable interface that can be tested in isolation.

Confirm the module design with the user. Ask which modules need tests.

## Step 5: Write the PRD

Synthesize everything into a PRD using the template below.

Before writing, ask the user where they want the PRD delivered. Options:

- **Markdown file** in the repo (suggest a sensible path)
- **GitHub issue** via `gh`
- **Other** — check for available MCP integrations (Notion, Linear, Shortcut, etc.) and offer any that are connected

<prd-template>

## Problem

What problem are we solving and why does it matter? Write from the user's perspective.

## Solution

High-level description of what we're building. Keep it concise — the details go in the sections below.

## Requirements

A numbered list of concrete, declarative requirements. Each should describe a specific behavior or capability. Write them so both humans and agents can unambiguously verify whether they're met.

Bad: "As a user, I want to filter results so that I can find what I need"
Good: "Users can filter search results by date range, status, and category"

1. ...
2. ...

## Technical Design

Key technical decisions, including:

- Modules to build or modify and their interfaces
- Architectural decisions and rationale
- Schema changes
- API contracts
- Important interactions between components

Do NOT include specific file paths or code snippets — they go stale quickly.

## Testing

- What makes a good test for this feature (test behavior, not implementation)
- Which modules need test coverage
- Existing test patterns in the codebase to follow

## Out of Scope

What this PRD explicitly does not cover.

## Open Questions

Unresolved decisions or areas that need further investigation. Remove this section if empty.

</prd-template>
