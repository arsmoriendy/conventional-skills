---
name: "general-agent-conventions"
description: "Project-agnostic working rules for coding agents: structured options for decisions, no running builds/linters, no write git commands, atomic 50-char commit suggestions, small diffs. Use on any coding, repo, git or commit task."
---

# General agent conventions

Project-agnostic working rules. A project's own instructions (CLAUDE.md, AGENTS.md, README) add specifics on top; these apply everywhere. If a project rule conflicts with one here, the project rule wins.

## Asking the user for decisions

When choosing between known alternatives, present **structured options** instead of an open-ended question: a concrete label, a one-line description each, with the recommended option first. This lets the user decide in one glance. Keep open-ended questions for clarifying _what_ to build, where there are no known alternatives yet.

## Commands and tooling

- **The user runs the app, the linter, and builds.** Don't start dev servers, run builds, or run linters unless asked. They have their own environment and watchers running, and extra processes collide with them.
- **Install dependencies in one checkout only.** Never install in a worktree: it duplicates dependencies and risks churning the lockfile. If dependencies look missing there, ask.
- **Run the project formatter after structural edits** (moving, splitting or renaming code), so diffs stay clean.

## Git and commits

Commits are the user's call, so never run `git commit` or any other write git command (`git add`, `git stash`, `git reset`, `git checkout` that changes files, and so on). Read-only git (`status`, `diff`, `log`) is fine.

Instead, **suggest an atomic conventional commit message**:

- Whole subject line max **50 characters**, e.g. `fix(parser): clamp line width`.
- Use `doc` (not `docs`) as the type; the shorter type frees up characters.
- **At most one suggestion per response.** The only exception is Plan mode, where the planned commits are listed as part of the plan.

How this plays out per mode:

- **Plan mode:** lay out the whole change set. If it isn't atomic, list one message per planned commit and say which change comes first.
- **Build mode:** implement exactly one atomic change, suggest its single message, and stop. Wait for the user to commit or reject it, then continue with the next change only when they say so. Never bundle several messages into one reply, even when the remaining changes are obvious; the next message comes after the next change.
- **No hidden queue:** never imply a change is still coming. When the last queued change is done, say so plainly, e.g. `All changes for "<initial prompt>" done.`, filling in the user's original request. Don't say you'll "pick up the next change" unless one is actually queued.

## Change discipline

- Prefer small additive diffs. Propose legacy cleanup separately rather than mixing it in, so each change stays reviewable and commit-sized.
- Reuse existing helpers and types instead of defining near-duplicates.
- Declutter: hoist inline handlers and nested helpers into named declarations before the `return`. Prefer a few small files over one large one.

## Keeping these rules current

When you learn something durable and non-obvious (a convention, gotcha, command, or user correction) that applies across projects, tell the user and offer to update this skill with a concise entry under the matching section, pruning anything stale. Skip task-specific trivia. Don't edit installed skill files directly; they are a read-only copy, so changes go through a skill update proposal.
