# Feature Development Loop

Every feature goes through the same loop -- from idea to production. This process is not bureaucracy. It is the difference between shipping something that works and shipping something that seems to work.

---

## The Loop

```
  1. Define     ->   2. Test first   ->   3. Implement
     the story          (TDD)              (red -> green -> refactor)
       v                                        v
  6. CI gates   <-   5. Review       <-   4. Document
  (must pass)
       v
  7. Ship & monitor
```

---

## Phase 1: Define the Story

Before writing a line of code, define what you are building and why. A story without acceptance criteria is just a wish.

**Template** -- save to `plans/stories/US-NNN-short-title.md`:

```markdown
# US-042: Short Title

## Story

**As a** [role]
**I want** [capability]
**So that** [benefit]

## Context

[Why does this matter? What happens without it?]

## Acceptance Criteria

### AC1: [Name]
**Given** [precondition]
**When** [action]
**Then** [expected result]

### AC2: [Name]
...

## Business Rules

- BR1: [Rule]
- BR2: [Rule]

## Definition of Done

- [ ] All acceptance criteria pass
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] CHANGELOG updated
- [ ] Code reviewed and merged
```

A good story answers: who benefits, what they can do, and why it matters. If you can't write acceptance criteria, you don't understand the feature well enough to build it yet.

---

## Phase 2: Write Tests First

Write tests before writing implementation. This forces you to think about the interface before the internals, and gives you a clear, failing target to work toward.

**Start with end-to-end tests** -- one per acceptance criterion:

```
test "AC1: [description of what AC1 verifies]" {
  // Arrange -- set up the precondition from the story
  // Act     -- perform the action from the story
  // Assert  -- verify the expected result from the story
}
```

**Then write unit tests** for the business logic:

```
test "calculates discount correctly for verified members" {
  // Arrange
  user  = User(type: "member", verified: true)
  price = 100

  // Act
  result = calculateDiscount(price, user)

  // Assert
  assert result.finalPrice == 90
  assert result.discountType == "member"
}

test "no discount for unverified members" {
  user  = User(type: "member", verified: false)
  result = calculateDiscount(100, user)
  assert result.finalPrice == 100
}
```

All tests should fail at this point. That's the point.

---

## Phase 3: Implement (Red -> Green -> Refactor)

Now write the code. Follow the cycle strictly:

1. **Red** -- run the tests, confirm they fail
2. **Green** -- write the *minimum* code to make them pass
3. **Refactor** -- improve the code while keeping tests green

Don't skip the refactor step. Green tests with messy code is not done.

```
# Red: tests fail -- function doesn't exist yet

# Green: minimal implementation that passes
function calculateDiscount(price, user) {
  if user.type == "member" and user.verified {
    return { finalPrice: price * 0.90, discountType: "member" }
  }
  return { finalPrice: price, discountType: null }
}

# Refactor: extract constants, improve clarity, same behavior
MEMBER_DISCOUNT = 0.10

function calculateDiscount(price, user) {
  if isMemberDiscount(user) {
    return applyDiscount(price, MEMBER_DISCOUNT, "member")
  }
  return noDiscount(price)
}
```

Commit at each meaningful step. Small commits tell a story. Don't commit broken code.

---

## Phase 4: Update Documentation

Documentation is part of the feature. A feature without documentation is not done.

**What to update:**

- **User docs** (`docs/user/`) -- how to use the feature; written for the user, not the developer
- **Developer docs** (`docs/developer/`) -- how it works, edge cases, integration points
- **CHANGELOG** -- add an entry under `[Unreleased]`
- **README** -- if the feature changes the project's headline capabilities

**User doc example:**

```markdown
# [Feature Name]

[One sentence: what this does and why you'd use it.]

## How to use it

1. Step one
2. Step two
3. Step three

## Troubleshooting

**Problem:** [Common issue]
**Solution:** [How to fix it]
```

**CHANGELOG entry:**

```markdown
## [Unreleased]

### Added
- [Feature name] -- [one sentence description] (#42)
```

---

## Phase 5: Self-Review

Before requesting a review, review your own work:

- [ ] All acceptance criteria are covered by tests
- [ ] All tests pass locally (`task test`)
- [ ] All checks pass locally (`task validate`)
- [ ] No debug statements, commented-out code, or stray TODOs
- [ ] Complex logic has comments explaining *why*, not *what*
- [ ] Public APIs have documentation comments
- [ ] User and developer docs updated
- [ ] CHANGELOG updated
- [ ] PR description is complete

---

## Phase 6: Code Review and CI

Open a pull request. Fill out the template. Link to the story.

CI must pass before merging:

1. **Format** -- code is formatted correctly
2. **Lint** -- no violations
3. **Type check** -- no type errors (if applicable)
4. **Test** -- all tests pass
5. **Build** -- project builds cleanly
6. **Security** -- no known vulnerabilities in dependencies

Address all review comments. Re-request review after making changes. Don't merge your own PR without approval.

---

## Phase 7: Ship and Monitor

After merging:

1. **Verify the deployment** -- confirm the change is live
2. **Smoke test** -- manually verify the feature works as expected
3. **Watch for errors** -- check logs for the first hour
4. **Measure impact** -- did the feature achieve what the story promised?

If something goes wrong, revert first and investigate second.

---

## Checklist

Copy this into your PR or story tracking:

```
- [ ] Story written with acceptance criteria
- [ ] Tests written for each AC (failing first)
- [ ] Unit tests written for business logic
- [ ] Implementation follows red-green-refactor
- [ ] User documentation updated
- [ ] Developer documentation updated (if needed)
- [ ] CHANGELOG updated
- [ ] Self-review completed
- [ ] All CI checks pass
- [ ] Code review approved
- [ ] Deployed and verified
```
