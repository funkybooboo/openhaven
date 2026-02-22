# Git Workflow

Clean Git history is not vanity -- it is a tool. A well-maintained history lets you understand why a change was made, bisect bugs to their source, and revert safely. Treat commits like documentation.

## Strategy: GitHub Flow

We use **GitHub Flow** -- simple, fast, and built for continuous delivery.

```
main --*------------------------------*-- (always deployable)
        \                            /
         *-- feature/thing ---------*
```

Rules:
- `main` is always deployable
- All changes go through pull requests
- Direct commits to `main` are not allowed
- Merging to `main` is the signal that something is ready to ship

---

## Branch Naming

```
<type>/<short-description-in-kebab-case>
```

| Type | Use for |
|------|---------|
| `feature/` | New features or enhancements |
| `bugfix/` | Bug fixes |
| `hotfix/` | Urgent production fixes |
| `refactor/` | Refactoring without behavior change |
| `docs/` | Documentation only |
| `test/` | Test additions or changes |
| `chore/` | Tooling, dependencies, maintenance |
| `spike/` | Research or experiments (not for merging) |

**Good:**
```
feature/user-authentication
bugfix/login-redirect-loop
docs/update-testing-philosophy
refactor/extract-validation-module
hotfix/patch-cve-2024-1234
```

**Bad:**
```
my-changes          # not descriptive
fix-bug             # which bug?
feature_user_auth   # use hyphens, not underscores
Feature/UserAuth    # use lowercase
temp                # not meaningful
```

---

## Conventional Commits

All commit messages must follow [Conventional Commits](https://www.conventionalcommits.org/). This is enforced by the `commit-msg` Git hook.

### Format

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Types

| Type | Use for |
|------|---------|
| `feat` | New feature |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `style` | Formatting, whitespace (no logic change) |
| `refactor` | Refactoring (no behavior change) |
| `perf` | Performance improvement |
| `test` | Adding or fixing tests |
| `build` | Build system or dependency changes |
| `ci` | CI/CD configuration |
| `chore` | Maintenance, tooling |
| `revert` | Reverts a previous commit |

### Examples

```
feat(auth): add OAuth2 login flow

Implements Google OAuth2 with authorization code exchange,
token refresh, and user profile mapping.

Closes #42
```

```
fix(storage): prevent duplicate key on concurrent writes

Add optimistic locking to the save operation. Previously,
two concurrent writes could both succeed and create duplicates.

Closes #87
```

```
docs: clarify no-mocks policy in testing guide
```

```
refactor(core): extract price calculation into domain module

Moves pricing logic out of the HTTP handler and into a pure
function in the domain layer. No behavior change.
```

### Breaking Changes

```
feat(api)!: change date format to ISO 8601

BREAKING CHANGE: All date fields now use ISO 8601 (YYYY-MM-DD)
instead of Unix timestamps. Clients must update their parsers.

Migration: docs/migrations/2024-01-date-format.md
```

### What Not to Write

```
# Bad
update stuff
fix bug
WIP
misc changes
feat: added some new features and also fixed a few bugs and updated docs
```

---

## Working on a Feature

```bash
# 1. Start from an up-to-date main
git checkout main
git pull origin main

# 2. Create your branch
git checkout -b feature/your-feature

# 3. Make small, focused commits as you work
git add -p                          # stage specific changes
git commit -m "feat: add X"

# 4. Keep your branch up to date
git fetch origin
git rebase origin/main              # prefer rebase over merge

# 5. Clean up history before opening PR
git rebase -i origin/main           # squash WIP commits, fix messages

# 6. Push and open PR
git push -u origin feature/your-feature
```

---

## Keeping History Clean

### Atomic Commits

Each commit should represent one complete, logical change. It should:
- Pass all tests on its own
- Have a meaningful commit message
- Not mix unrelated changes

### Interactive Rebase

Before opening a PR, clean up your commit history:

```bash
git rebase -i origin/main
```

In the editor:
- `pick` -- keep as-is
- `reword` -- change the commit message
- `squash` -- combine with the previous commit (keeps both messages)
- `fixup` -- combine with the previous commit (discards this message)
- `drop` -- remove the commit entirely

Use this to:
- Squash "fix typo" and "WIP" commits into the commit they belong to
- Reorder commits for logical flow
- Rewrite unclear commit messages

**Never rebase commits that have been pushed to a shared branch.**

### Patch Staging

Stage specific changes within a file -- not the whole file:

```bash
git add -p
```

This lets you make multiple logical changes in one working session and commit them separately.

---

## Pull Requests

### Title

PR titles must follow Conventional Commits format -- they become the squash commit message.

```
feat(auth): add OAuth2 login flow
fix(storage): prevent duplicate key on concurrent writes
docs: clarify no-mocks policy in testing guide
```

### Size

Keep PRs small. A PR that can be reviewed in one sitting gets reviewed faster and gets better feedback. Aim for under 400 lines of changed code.

If a feature is large, break it into a sequence of smaller PRs that each stand on their own.

### Merge Strategy

- **Squash merge** -- default for feature branches. Combines all commits into one clean commit on `main`.
- **Rebase merge** -- for branches with a clean, well-crafted commit series worth preserving.

---

## Git Hooks

Two hooks run automatically:

**`commit-msg`** -- validates commit message format (Conventional Commits). Runs on every commit.

**`pre-commit`** -- runs fast checks before every commit:
- Code formatter check
- Linter

Keep this fast -- it runs on every commit. Slow checks (full test suite, build) belong in CI.

If a hook fails, fix the issue and try again. Never skip hooks with `--no-verify` unless you have a very good reason.

---

## Useful Commands

```bash
# Undo last commit, keep changes staged
git reset --soft HEAD~1

# Undo last commit, keep changes unstaged
git reset HEAD~1

# Find which commit introduced a bug
git bisect start
git bisect bad                    # current commit is broken
git bisect good <known-good-hash> # last known good state
# git checks out commits for you to test
git bisect good   # or: git bisect bad
git bisect reset  # when done

# Save work in progress to switch context
git stash push -m "WIP: description of what you were doing"
git stash pop     # restore when you come back

# Apply a specific commit from another branch
git cherry-pick <commit-hash>

# See what changed and why
git log --oneline --graph
git log -p <file>               # history of a specific file
git blame <file>                # who changed each line and when
```

---

## Protected Main Branch

Configure these branch protection rules on GitHub:

- [OK] Require pull request before merging
- [OK] Require at least 1 approval
- [OK] Dismiss stale approvals when new commits are pushed
- [OK] Require status checks to pass (all CI jobs)
- [OK] Require branches to be up to date before merging
- [OK] Require conversation resolution before merging
- [OK] Do not allow force pushes
- [OK] Do not allow deletions

---

## Summary

| Do | Don't |
|----|-------|
| Keep commits small and focused | Commit directly to `main` |
| Write meaningful commit messages | Use "WIP", "fix", "update" as messages |
| Rebase to clean history before PR | Force push to shared branches |
| Keep PRs under 400 lines | Mix unrelated changes in one commit |
| Link PRs to issues | Push secrets or credentials |
| Respond to all review comments | Merge your own PR without approval |
| Update tests and docs with code | Ignore CI failures |
