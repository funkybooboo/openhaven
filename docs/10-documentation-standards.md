# Documentation Standards

Documentation is a first-class citizen. It lives in the repository, is reviewed like code, and is updated alongside code. Docs that live outside the repo rot.

- **Plain text in Git** -- Markdown, version controlled, searchable, reviewable
- **Updated with code** -- a PR that changes behavior updates the docs
- **Two audiences** -- user docs explain how to use it; developer docs explain how it works
- **Clarity over completeness** -- a short, accurate doc beats a long, stale one
- **Write for the reader** -- not to prove you understand it

---

## Why Plain Text in Git?

1. **Version controlled** -- see what changed, when, and why
2. **Reviewed in PRs** -- documentation errors are caught before they ship
3. **Searchable** -- `grep` works; no special tools required
4. **Portable** -- readable in any editor, on any machine, offline
5. **Future-proof** -- plain text outlives every tool and platform

Never store docs in Google Docs, Notion, Confluence, or any external tool. Never use proprietary formats. Never keep docs in a separate repository from the code.

---

## Directory Structure

```
project/
|-- docs/
|   |-- 00-table-of-contents.md
|   |-- 01-introduction.md
|   |-- ...
|   |-- user/           <- end-user documentation
|   `-- developer/      <- feature-specific developer guides
|
|-- plans/
|   |-- roadmap.md
|   |-- stories/        <- user stories (US-NNN-title.md)
|   |-- specs/          <- technical specs for complex features
|   |-- decisions/      <- architecture decision records (ADR-NNN-title.md)
|   `-- retrospectives/ <- release retrospectives
|
|-- README.md           <- front door: what, quick start, links
|-- CHANGELOG.md        <- version history
|-- CONTRIBUTING.md     <- how to contribute
`-- SECURITY.md         <- vulnerability reporting
```

---

## Two Audiences

Every feature should be documented from two perspectives.

### User documentation (`docs/user/`)

Written for people who use the project without reading the source.

- What can I do?
- How do I do it?
- What do I do when it breaks?

Keep it task-oriented. No implementation details. No jargon.

```markdown
# Exporting Data

Export your records to a file for use in other tools.

## Basic export

    app export

Creates `data.csv` in the current directory.

## Custom output path

    app export --output /path/to/file.csv

## Troubleshooting

**Problem:** Permission denied
**Solution:** Check that you have write access to the output directory.
```

### Developer documentation (`docs/developer/`)

Written for contributors and maintainers.

- How does it work?
- Why was it built this way?
- What are the edge cases?
- Where are the tests?

```markdown
# Export Feature

## How it works

The `export` command reads from the `Storage` port and writes through
the `FileWriter` port. Both are injected, so both can be swapped in tests.

## Edge cases

- Fields containing delimiters are quoted per RFC 4180
- Dates are always ISO 8601 regardless of locale
- Empty result sets produce a header-only file (not an error)
- Existing files are overwritten with a warning to stderr

## Tests

- Unit: formatting logic in `test_formatter`
- Integration: file writing in `test_export_command`
- E2E: full command in `test_export`
```

---

## Architecture Decision Records

When you make a significant architectural decision, write it down. Future contributors -- including yourself -- will want to know why things are the way they are.

**Location:** `plans/decisions/ADR-NNN-short-title.md`

```markdown
# ADR-001: Use In-Memory Storage for Tests

## Status

Accepted

## Context

We need a storage implementation for tests that doesn't require
a running database.

## Decision

Use an in-memory implementation that satisfies the Storage interface.

## Rationale

- Zero setup -- no database required to run tests
- Fast -- no I/O overhead
- Real behavior -- unlike mocks, it actually stores and retrieves data

## Consequences

### Positive
- Tests run in milliseconds
- No external dependencies for unit and integration tests

### Negative
- Must keep InMemoryStorage in sync with real storage behavior

## Alternatives Considered

- **Mocks** -- rejected; tests implementation details, not behavior
- **Real database in container** -- use for E2E tests, not unit/integration
```

Write an ADR when:
- Choosing between two non-obvious approaches
- Making a decision that will be hard to reverse
- Adopting or rejecting a technology, library, or pattern
- Establishing a convention others will follow

---

## Markdown Standards

### Structure

- One H1 (`#`) per file -- the document title
- H2 (`##`) for major sections
- H3 (`###`) for subsections
- Never skip levels

### Lists

Use `-` for unordered lists. Use `1.` for ordered steps.

### Code blocks

Specify the language when it's real code. Use unlabeled blocks for pseudocode and command examples.

````markdown
```bash
task test
```

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"
```

```
# Pseudocode -- no language tag needed
result = find_user(id)
if result is null -> raise NotFoundError
```
````

### Links

Use descriptive link text -- never "click here":

```markdown
# Good
See the [testing guide](./05-testing.md) for details.

# Bad
Click [here](./05-testing.md) for more information.
```

---

## CHANGELOG

Every user-visible change gets an entry. Follow [Keep a Changelog](https://keepachangelog.com/).

```markdown
## [Unreleased]

### Added
- `export` command writes records to CSV (#42)

### Changed
- Config now uses TOML instead of YAML (#38)

### Fixed
- Crash on empty input file (#51)

### Removed
- Deprecated `--legacy` flag; use `--compat` instead (#44)
```

Rules:
- Add entries under `[Unreleased]` as you work
- Move `[Unreleased]` to a version heading on release
- Write for users -- describe the impact, not the implementation
- Link to the issue or PR

---

## README

The README answers four questions in under two minutes:

1. **What is this?** -- one sentence
2. **Why does it exist?** -- the problem it solves
3. **How do I get started?** -- minimum steps to run it
4. **Where do I go next?** -- links to docs, contributing guide

Keep it short. Link to the detailed docs. Don't duplicate content that lives elsewhere.

---

## Checklist

Before merging any PR that changes behavior:

- [ ] User docs updated (if the change is user-visible)
- [ ] Developer docs updated (if the implementation changed significantly)
- [ ] CHANGELOG entry added under `[Unreleased]`
- [ ] Examples in docs are accurate
- [ ] Links are valid
- [ ] Written for the reader, not the author
