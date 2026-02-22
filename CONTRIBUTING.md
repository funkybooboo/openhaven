# Contributing to OpenHaven

Thanks for your interest in contributing to OpenHaven!

OpenHaven is in early development (v0.1.0 - Documentation Phase). We're currently establishing the vision, architecture, and planning foundation before writing code.

## Current Stage: Documentation & Planning

Right now, the most valuable contributions are:

### 1. Vision & Philosophy Feedback

Read and provide feedback on:
- [Introduction](./docs/01-introduction.md) - What OpenHaven is and why it exists
- [Vision](./plans/vision.md) - Long-term vision and strategic positioning
- [Architecture](./docs/03-architecture.md) - System design and principles

**How to contribute:**
- Open issues with questions, concerns, or suggestions
- Propose improvements to the vision or philosophy
- Share your self-hosting pain points

### 2. Architecture & Design

Review and discuss:
- [ADR-001: User-Owned Infrastructure](./plans/decisions/ADR-001-user-owned-infrastructure.md)
- [ADR-002: No SaaS Dependencies](./plans/decisions/ADR-002-no-saas-dependencies.md)
- [Stack Decisions](./plans/specs/stack-decisions.md)

**How to contribute:**
- Challenge architectural decisions with evidence
- Propose alternative approaches
- Share experience with similar systems

### 3. Documentation Improvements

Help improve:
- Clarity and readability
- Technical accuracy
- Examples and use cases
- Missing sections

**How to contribute:**
- Fix typos and grammar
- Add examples
- Clarify confusing sections
- Fill in TODOs

### 4. Planning & Roadmap

Review:
- [Roadmap](./plans/roadmap.md)
- User stories (coming soon)
- Technical specifications (coming soon)

**How to contribute:**
- Suggest features or use cases
- Identify missing requirements
- Propose roadmap adjustments

## Future Contributions (v0.2.0+)

Once we move to the implementation phase, we'll need:
- Terraform modules
- Docker Compose templates
- Control plane development
- Testing and validation
- Documentation updates

The full contributing guide will be available at [docs/06-contributing.md](./docs/06-contributing.md).

## Quick Reference

```bash
# Clone the repository
git clone https://github.com/funkybooboo/openhaven.git
cd openhaven

# Read the documentation
cat docs/01-introduction.md
cat plans/vision.md
cat docs/03-architecture.md

# Commit format (when contributing)
git commit -m "type(scope): description"
# types: docs feat fix refactor test chore
```

## Code of Conduct

This project follows the [Contributor Covenant](./CODE_OF_CONDUCT.md). Be respectful and constructive.
