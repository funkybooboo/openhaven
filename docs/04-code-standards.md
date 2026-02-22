# Code Standards

Code is read far more often than it is written. Write for the next person who reads it -- which is usually you, six months from now.

- **Explicit over implicit** -- make intent clear through names, types, and structure
- **Readable over clever** -- obvious code beats elegant code that requires study
- **Consistent** -- follow the patterns already in the codebase

---

## Naming

Good names are the most powerful tool for making code readable. Spend time on them.

### Functions and methods

Use verbs or verb phrases that describe what the function does.

```
# Good
calculate_total(items)
validate_email(address)
find_user_by_id(id)
is_authenticated(token)
has_permission(user, action)

# Bad
total(items)      -- noun, not a verb
data(input)       -- meaningless
user(id)          -- ambiguous
valid(email)      -- missing predicate form
```

### Variables

Describe what the value represents, not its type or how it was obtained.

```
# Good
user_count        = count(users)
is_authenticated  = check_auth(token)
max_retries       = 3

# Bad
n    = count(users)       -- too terse
flag = check_auth(token)  -- meaningless
x    = 3                  -- no context
```

### Booleans

Boolean names should read as yes/no questions.

```
# Good          # Bad
is_active       active
has_permission  permission
can_edit        edit
should_retry    retry
```

### Constants

Name the meaning, not the value.

```
# Good                              # Bad
MAX_RETRY_ATTEMPTS = 3              THREE = 3
DEFAULT_TIMEOUT_MS = 5000           FIVE_SECONDS = 5000
```

### Abbreviations

Write full words. Only abbreviate what is universally understood.

**Acceptable:** `id`, `url`, `api`, `http`, `config`, `auth`, `max`, `min`, `err`, `arg`

```
# Good                  # Bad
user_message            usr_msg
error_handler           err_hndlr
request_context         req_ctx
```

---

## Functions

### Single responsibility

Each function does one thing. If you need "and" to describe it, split it.

### Small and focused

If a function doesn't fit on one screen, consider breaking it up.

### Explicit parameters

Pass dependencies in. Don't reach out and grab them from global state.

```
# Good -- dependency is visible
calculate_discount(price, user_type)

# Bad -- hidden dependency
calculate_discount(price)
  user_type = get_current_user().type  -- where did this come from?
```

### Explicit return types

Be clear about what a function returns. If it can fail, make that visible in the signature.

---

## Error Handling

### Never silently swallow errors

```
# Good
try:
  result = risky_operation()
except SpecificError as err:
  log.error("operation failed", error=err)
  raise

# Bad
try:
  result = risky_operation()
except:
  pass  -- silent failure; the bug disappears
```

### Use specific error types

Define error types for different failure modes. A specific error communicates more than a generic one.

```
# Good
raise ValidationError("email is required", field="email")
raise NotFoundError("user not found", id=user_id)

# Bad
raise Error("something went wrong")
```

### Fail fast

Validate inputs at the boundary. Don't let invalid data propagate deep into the system where the error message will be confusing.

---

## Comments

### Comment why, not what

The code shows what is happening. Comments explain why.

```
# Good -- explains a non-obvious decision
# Cap at 90 days -- older records are archived and excluded
# from live queries to keep response times under 100ms
cutoff = today() - days(90)

# Bad -- restates the code
# subtract 90 days from today
cutoff = today() - days(90)
```

### When to comment

**Do comment:**
- Non-obvious design decisions
- Workarounds for external bugs or limitations
- Business rules that aren't obvious from the domain
- Complex algorithms (with a reference if applicable)

**Don't comment:**
- Obvious code
- What is already clear from names
- Code that should be refactored instead of explained

### Document public APIs

All public functions, types, and modules should have documentation comments covering: what it does, parameters, return value, errors it can raise, and a usage example if helpful.

---

## File and Module Organization

### One concept per file

Each file has a clear, single purpose. The filename makes that purpose obvious.

```
# Good                      # Bad
user_repository             utils
email_validator             helpers
order_calculator            misc
```

### Avoid circular dependencies

If module A imports module B and module B imports module A, the design is wrong. Extract the shared concept into a third module.

---

## Style

### Use a formatter

Automate style decisions. Configure a formatter for your language and run it on every commit. Don't debate whitespace in code reviews.

| Language | Formatter |
|----------|-----------|
| Rust | `rustfmt` |
| Go | `gofmt` |
| Python | `ruff format` or `black` |
| JavaScript / TypeScript | `prettier` or `biome` |
| C / C++ | `clang-format` |
| Swift | `swift-format` |

### Use a linter

Linters catch bugs and enforce patterns that formatters don't cover.

| Language | Linter |
|----------|--------|
| Rust | `clippy` |
| Go | `golangci-lint` |
| Python | `ruff` |
| JavaScript / TypeScript | `eslint` or `biome` |

---

## Type Safety

Use the type system your language provides. Types are documentation that the compiler checks.

- Prefer domain types over primitives -- `UserId` is clearer than a bare `string`
- Make invalid states unrepresentable through the type system
- Handle nullable / optional values explicitly; don't assume non-null
- Avoid casts and type assertions; document why when you must use them

---

## Enforcement

Standards are enforced through:

1. **Formatter** -- runs on commit via pre-commit hook; no manual style debates
2. **Linter** -- runs in CI; violations fail the build
3. **Code review** -- reviewers check for adherence to these standards
4. **Tests** -- code that violates these standards is usually harder to test, which surfaces problems early

Run all checks before pushing:

```bash
task validate
```
