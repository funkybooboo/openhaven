# Technical Specifications

Detailed technical plans for complex features. Not every story needs a spec -- write one when the implementation involves significant design decisions, multiple components, or non-obvious tradeoffs.

Name files to match their story: `US-NNN-short-title.md`.

## When to Write a Spec

Write a spec when:
- The feature touches multiple layers or components
- There are meaningful architectural decisions to make
- The implementation approach isn't obvious
- You want to get feedback on the approach before writing code

Skip the spec when:
- The implementation is straightforward
- The story's acceptance criteria are sufficient guidance

## Spec Template

```markdown
# Spec: [Feature Name] (US-NNN)

## Overview

[One paragraph: what this feature does and how it fits into the system]

## Architecture

[Diagram or description of how components interact]

## Implementation Plan

### Phase 1: [Name]
- [ ] Step 1
- [ ] Step 2

### Phase 2: [Name]
- [ ] Step 1
- [ ] Step 2

## Data Model Changes

[Any new types, schema changes, or storage changes]

## Interface Changes

[Any new ports, public APIs, or CLI commands]

## Edge Cases

- [Edge case 1 and how it's handled]
- [Edge case 2 and how it's handled]

## Testing Strategy

- Unit: [what to unit test]
- Integration: [what to integration test]
- E2E: [what to E2E test]

## Risks and Mitigations

| Risk | Likelihood | Mitigation |
|------|-----------|------------|
| [Risk] | Low/Med/High | [How to handle it] |

## Open Questions

- [ ] [Question that needs an answer before or during implementation]
```
