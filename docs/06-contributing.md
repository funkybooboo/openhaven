# Contributing

Thank you for taking the time to contribute. This document explains how to get involved -- whether you're fixing a bug, proposing a feature, or improving documentation.

## Ways to Contribute

- **Report a bug** -- open an issue with steps to reproduce
- **Request a feature** -- open an issue to discuss the idea first
- **Fix a bug** -- open a pull request referencing the issue
- **Add a feature** -- open an issue first, then a pull request
- **Improve docs** -- documentation PRs are always welcome

---

## Before You Start

For anything beyond a small fix, **open an issue first**. This lets us discuss the approach before you invest time writing code. Nothing is worse than finishing a PR only to find out it conflicts with the roadmap or was already solved differently.

For bugs: describe what you expected, what happened, and how to reproduce it.

For features: describe the problem you're solving and why it belongs in this project.

---

## Development Setup

See [02: Getting Started](./02-getting-started.md) for full setup instructions.

Quick version:

```bash
git clone https://github.com/username/project-name.git
cd project-name
task setup
```

---

## Making Changes

### 1. Create a branch

Branch from `main`. Use a descriptive name:

```bash
git checkout main
git pull origin main
git checkout -b feature/your-feature-name
```

Branch naming:

| Prefix | Use for |
|--------|---------|
| `feature/` | New features |
| `bugfix/` | Bug fixes |
| `docs/` | Documentation only |
| `refactor/` | Refactoring without behavior change |
| `test/` | Adding or fixing tests |
| `chore/` | Tooling, dependencies, maintenance |

### 2. Write tests first

Before writing implementation code, write a failing test that describes the behavior you want. See [05: Testing](./05-testing.md) for the full testing philosophy.

### 3. Make your changes

- Follow the [code standards](./04-code-standards.md)
- Keep commits small and focused -- one logical change per commit
- Write commit messages in [Conventional Commits](https://www.conventionalcommits.org/) format

### 4. Validate locally

Before pushing, run the full validation suite:

```bash
task validate
```

This runs formatting, linting, type checking, and tests. Fix everything before pushing -- don't rely on CI to catch things you could catch locally.

### 5. Open a pull request

Push your branch and open a PR against `main`. Fill out the PR template completely. Link to the issue it resolves.

---

## Commit Messages

We use [Conventional Commits](https://www.conventionalcommits.org/). This is enforced by a Git hook -- commits that don't follow the format will be rejected.

Format:

```
<type>(<scope>): <short description>

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

Examples:

```
feat(auth): add token refresh logic

fix(storage): handle concurrent write race condition

docs: clarify testing philosophy in 05-testing.md

refactor(core): extract validation into separate module

test(api): add coverage for 404 and 422 error paths
```

---

## Pull Request Standards

A good PR:

- Has a clear title in Conventional Commits format
- Describes what changed and why (not just what)
- Links to the issue it resolves (`Closes #123`)
- Includes tests for all new behavior
- Updates documentation if behavior changed
- Passes all CI checks
- Is small enough to review in one sitting (aim for under 400 lines)

If your change is large, consider breaking it into smaller PRs that can each be reviewed and merged independently.

---

## Code Review

Reviews exist to improve the code, not to gatekeep. Be kind. Be specific. Be constructive.

**As an author:**
- Respond to every comment
- Don't take feedback personally -- it's about the code
- Ask for clarification if a comment is unclear
- Mark resolved comments as resolved

**As a reviewer:**
- Explain the why behind requested changes
- Distinguish between blocking issues and suggestions
- Approve when the code is good enough -- perfect is the enemy of shipped
- Review the tests as carefully as the implementation

---

## What Gets Merged

A PR is ready to merge when:

- All CI checks pass
- At least one approval from a maintainer
- All review comments are resolved
- The branch is up to date with `main`

---

## Reporting Security Issues

Do not open a public issue for security vulnerabilities. See [SECURITY.md](../SECURITY.md) for the responsible disclosure process.

---

## Code of Conduct

This project follows the [Contributor Covenant](../CODE_OF_CONDUCT.md). Be respectful. Be inclusive. Focus on the work.
