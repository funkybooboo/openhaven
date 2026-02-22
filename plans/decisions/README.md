# Architecture Decision Records

Every significant architectural decision gets a record here. ADRs are short, permanent, and written at the time the decision is made.

Name files `ADR-NNN-short-title.md`. Never delete or edit a past ADR -- if a decision is reversed, write a new ADR that supersedes it.

## Status Values

| Status | Meaning |
|--------|---------|
| `Proposed` | Under discussion, not yet decided |
| `Accepted` | Decision made, in effect |
| `Superseded by ADR-NNN` | Replaced by a later decision |
| `Deprecated` | No longer relevant |

## Index

| ADR | Title | Status |
|-----|-------|--------|
| [ADR-001](./ADR-001-example.md) | Example ADR (delete me) | Accepted |

---

## ADR Template

```markdown
# ADR-NNN: [Short Title]

## Status

[Proposed | Accepted | Superseded by ADR-NNN | Deprecated]

## Context

[What situation prompted this decision? What options were available?]

## Decision

[What did we decide?]

## Rationale

- [Reason 1]
- [Reason 2]
- [Reason 3]

## Consequences

### Positive
- [Benefit 1]
- [Benefit 2]

### Negative
- [Tradeoff 1]
- [Tradeoff 2]

## Alternatives Considered

1. **[Option A]**: [Why it was rejected]
2. **[Option B]**: [Why it was rejected]
```
