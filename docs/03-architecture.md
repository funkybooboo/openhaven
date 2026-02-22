# Architecture

This document covers how to think about structuring a project -- the principles that lead to code that is easy to test, change, and understand. These ideas apply regardless of language, framework, or project type.

---

## The Core Principle: Separate What from How

The most important architectural decision you will make is where to draw the line between **core logic** and **infrastructure**.

- **Core logic** is *what* your program does -- the rules, decisions, and transformations that make it valuable.
- **Infrastructure** is *how* it does it -- files, networks, databases, terminals, hardware, external services.

Core logic should not know about infrastructure. Infrastructure should be swappable without touching core logic.

---

## Ports and Adapters

One proven way to enforce this separation: define **ports** (interfaces that describe what your core needs) and implement **adapters** (concrete implementations that satisfy those interfaces).

```
+----------------------------------------------+
|                                              |
|              Core Logic                      |
|       (rules, decisions, transforms)         |
|                                              |
|   depends on v interfaces, not details       |
|                                              |
|--------------+-------------------------------+
|   Storage    |   Logger   |   Clock   | ...  |  <- Ports (interfaces)
|--------------+-------------------------------+
|  Postgres  InMemory  File  Console  System   |  <- Adapters (implementations)
+----------------------------------------------+
```

**A port** is an interface -- a contract that describes a capability your core needs, without specifying how it's provided.

**An adapter** is a concrete implementation of a port. You can have multiple adapters for the same port:

| Port | Production adapter | Test adapter |
|------|--------------------|--------------|
| Storage | Database | In-memory map |
| Logger | Structured JSON | Silent / recording |
| Clock | System time | Fixed / controllable |
| Config | Environment vars | Hardcoded map |
| Notifier | Email / SMS | Recording (captures calls) |

This pattern is sometimes called Hexagonal Architecture or Clean Architecture. The name doesn't matter. The idea does: **keep your core logic independent of the outside world.**

---

## Dependency Injection

Adapters need to be wired together somewhere. Do it in one place -- a composition root, a container, or your `main` entry point.

```
main / container
  |-- creates Config  (reads from environment)
  |-- creates Logger  (uses Config for log level)
  |-- creates Storage (uses Config for connection string)
  `-- creates App     (receives Storage, Logger -- knows nothing about how they work)
```

**Rules:**
- Construct dependencies once, at startup
- Pass them in -- don't reach out and grab them from globals
- The composition root is the only place that knows which concrete adapter is used

---

## 12-Factor Principles

Regardless of project type, these principles lead to software that is portable, maintainable, and honest:

1. **Config in the environment** -- no hardcoded values; read from env vars, flags, or config files; never commit secrets
2. **Explicit dependencies** -- declare everything; don't rely on ambient globals or system-installed tools
3. **Stateless processes** -- don't store state in memory between invocations; state belongs in storage
4. **Logs to stdout** -- write log output as a stream of events; let the environment handle routing
5. **One codebase, many environments** -- the same code runs in dev, test, and production; only config changes

See [12factor.net](https://12factor.net/) for the full list.

---

## Architecture Decision Records

When you make a significant architectural decision, document it. Future contributors -- including yourself -- will want to know why things are the way they are.

ADRs live in [`plans/decisions/`](../plans/decisions/). See the [README there](../plans/decisions/README.md) for the template and naming convention.

Write an ADR when:
- Choosing between two non-obvious approaches
- Making a decision that will be hard to reverse
- Adopting or rejecting a technology, library, or pattern
- Establishing a convention others will follow

---

## What to Avoid

- **God objects** -- one class or module that knows everything and does everything
- **Hidden dependencies** -- global state, singletons grabbed without injection
- **Infrastructure in core logic** -- importing a database driver inside a business rule
- **Premature abstraction** -- don't create interfaces for things that will never have a second implementation
- **Inheritance for code reuse** -- prefer composition; inheritance couples you to the parent's internals
