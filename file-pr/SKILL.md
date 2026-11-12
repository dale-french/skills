---
name: file-pr
description: File a concise pull request. Use when the user asks to file, open, or create a PR.
tags: [pull-request, engineering]
author: Dale French (@dale-french)
---

# File PR

## PR Title

The conventional title format is `[shortcut-story-id] Shortcut story title`. If no shortcut story is available, derive a title from the branch name and diff. The title should be concise and clear.

## PR Body

Use this template for writing the PR body:

```markdown
## Summary

<summary>

## Evidence

**Before:** <command run on <base>, with its output>

**After:** <the same command on this branch, with its output>

## Merge Danger

**Door:** <one-way or two-way>

<optional: description>

**Blast Radius:** <one-word description>

<optional: potential ramifications of merge>
```

### Sections

Skip all preambles and keep prose brief. Apply the `writing` skill's prose rules.

### Summary

A simple explanation of the problem based on the user's original prompt, then briefly explain the solution. Do not lead with an implementation inventory. Pick the smallest view that makes the key point clear.

### Evidence

Evidence is what you ran in this session, with its real output. Never describe
what the code "would" do unless you are not able to validate it. If you cannot validate, explain why and what you would do to validate it.

Before you file, validate:

1. Run the same check on the base branch and on this branch, with the same
   input (`git show <base>:<file>`, a stub, or a test). That gives the Before
   and the After.
2. Prefer real input (production logs, a real report, a recorded response).
   Use hand-written input only for cases the real data doesn't contain, and
   say which results came from it.
3. Show the command as pseudocode or the QA steps and bullet points, cut to what proves the point.
4. Name what you did not cover, and the step that would cover it, such as the
   first CI run after merge.

Screenshots are S-tier when the change is visual and the environment allows it.
Execution-based evidence is A-tier.

When /file-pr is invoked, stop before you create or edit the PR. Ask the user
with AskUserQuestion. Show what you have run so far, what you would run to
validate, and the data or access it needs. Options: "Validate, then file
(Recommended)", "File with the current evidence", "I'll give you the
validation steps". Do not file until the user answers.

### Merge Danger

Describe whether it's a one-way or two-way door. You can walk back through two-way doors, but not one-way doors. A PR that is cheap to roll back is lower risk. Changes that involve destructive actions or hard-to-reverse decisions are one-way doors.

The blast radius is the potential impact or scope of the changes introduced by this PR. Consider all possibilities. Examples are layout shift, breakages for consumers, mobile responsiveness, etc.

## General rules

- Always open the PR as a draft unless ask otherwise.
