# Git Guidelines

> A living draft. Add, remove, and reshape these rules as your workflow evolves.
> Last updated: 2026-07-28

## Purpose

These are the conventions I (Claude) follow when working with Git in this vault
and in code projects. Keep this file as the source of truth so behaviour stays
consistent across sessions. Edit anything that doesn't fit how you actually work.

## Golden rules

- Never commit or push unless explicitly asked. Making changes and committing are two separate steps.
- Never commit directly to `main`/`master`. Branch first, even for small fixes.
- One logical change per commit. If a commit needs the word "and" to describe it, split it.
- Never rewrite history that's already been pushed and shared (no `push --force` on shared branches).
- Never commit secrets, credentials, `.env` files, or large binaries. Add them to `.gitignore` instead.

## Branching

- Branch off an up-to-date `main`: `git switch main && git pull` first.
- Name branches with a type prefix and short description:
  - `feature/short-description`
  - `fix/short-description`
  - `docs/short-description`
  - `refactor/short-description`
- Keep branches short-lived. Merge or delete rather than letting them rot.

## Commit messages

Follow the Conventional Commits style:

```
<type>(<optional scope>): <short summary in imperative mood>

<optional body: what and why, not how>

<optional footer: issue refs, breaking changes>
```

Common types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`.

Examples:

```
feat(auth): add password reset flow
fix(parser): handle empty input without crashing
docs: expand git guidelines draft
```

Guidelines:

- Summary line under ~50 characters, no trailing period, imperative ("add", not "added").
- Explain *why* in the body when the reason isn't obvious from the diff.
- Reference issues in the footer: `Closes #42`.

## Before every commit

- Review what you're staging: `git status` then `git diff --staged`.
- Stage intentionally with `git add <path>` rather than `git add .` when possible.
- Make sure the project still builds / tests pass.
- Confirm no debug prints, commented-out junk, or secrets snuck in.

## Everyday workflow

```bash
git switch main
git pull
git switch -c feature/my-change
# ... make changes ...
git add <files>
git diff --staged        # sanity check
git commit -m "feat: my change"
git push -u origin feature/my-change
# open a pull request
```

## Pull requests

- Keep them small and focused — easier to review, faster to merge.
- Write a description that says what changed and why.
- Link related issues.
- Delete the branch after merging.

## Fixing mistakes

- Undo unstaged changes to a file: `git restore <file>`
- Unstage a file (keep the edits): `git restore --staged <file>`
- Amend the *most recent, unpushed* commit: `git commit --amend`
- Undo the last commit but keep the changes: `git reset --soft HEAD~1`
- Recover something you think you lost: `git reflog`

## Merge conflicts

- Don't panic — conflicts are normal.
- Open the conflicted files, resolve the `<<<<<<<` / `>>>>>>>` markers by hand.
- `git add` the resolved files, then continue the merge/rebase.
- When unsure, `git merge --abort` (or `git rebase --abort`) resets you to safety.

## .gitignore essentials

Always ignore, at minimum:

```
# Dependencies
node_modules/
# Environment / secrets
.env
.env.*
# Build output
dist/
build/
# OS / editor cruft
.DS_Store
.idea/
.vscode/
```

## Notes for this vault

<!-- Add project-specific conventions, repo URLs, or exceptions here as you go. -->

---

*This is a draft — keep evolving it.*
