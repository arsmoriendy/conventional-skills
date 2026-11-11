---
name: harness-tools
description: Use before running any shell command for repository work, including reading, listing, searching, or editing files. Prefers the harness's dedicated file tools (read, glob, grep, edit, write) over shell equivalents, and limits shell to commands that have no tool equivalent, such as build, test, lint, format, version control inspection, and package management. Apply this whenever you are about to reach for cat, ls, find, grep, sed, or similar to look at or change files, even for a quick one-off check.
---

# Harness Tools

Use the harness's dedicated tools for repository work, and keep the shell for
commands that have no tool equivalent. Dedicated tools give structured,
reviewable results, are easier for the user to approve and audit, and avoid the
quoting and escaping errors that shell one-liners invite.

## Rules

- Read files with the read tool, not `cat`, `head`, `tail`, `less`, or `wc`.
- List and find files with the glob tool, not `ls`, `find`, or `tree`.
- Search file contents with the grep tool, not `grep`, `rg`, or `ag`.
- Change files with the edit tool (targeted change) or the write tool (full
  replacement), not `sed`, `awk`, `perl -i`, `tee`, redirection, or heredocs.
- Do not use the shell to inspect or modify files. If no harness tool covers a
  case, say so rather than substituting a shell equivalent, so the user can
  decide how to proceed.

## Shell exceptions

Use the shell only for work the file tools cannot do:

- Build, type-check, test, lint, and format commands for the project's toolchain
- Running the program or its scripts, only when the user asks you to
- Read-only version control inspection (status, diff, log)
- Package manager, dependency, and preview or dev-server commands the user
  asked for

Prefer the project's own documented commands (check the README, Makefile,
package manifest, or CI config) over guessing.

## Output hygiene

Run one operation per call. Do not chain commands or insert separators such as
`echo "====";` or `printf '---'`. Each call's output stays clean and is easy to
attribute to the command that produced it.
