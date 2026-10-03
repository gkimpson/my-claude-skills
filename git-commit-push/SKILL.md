---
name: git-commit-push
description: Automated commit and push - stages changes, generates Conventional Commits message, commits without co-author, pushes to origin
---

Automate the full git workflow: check status, stage changes, generate a Conventional Commits message using the commit-message-writer agent, commit (no co-author), and push to origin/main.

## Procedure

1. Run `git status` to show unstaged changes
2. Run `git diff` to preview what will be committed
3. Stage all changes: `git add -A` (or specific files)
4. Use commit-message-writer agent to generate a Conventional Commits message based on the staged diff
5. Create commit with generated message, explicitly WITHOUT co-authored attribution
6. Push to origin main: `git push origin main`

## Key Requirements

- **Never add co-authored lines** — commit message is author-only
- Use commit-message-writer agent for Conventional Commits format
- Push immediately after committing
- Show user the final commit hash and confirmation
