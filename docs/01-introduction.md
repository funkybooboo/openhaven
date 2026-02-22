# Introduction

Welcome. This document explains what this project is, why it exists, and the values that guide how it's built.

---

## What Is This Project?

<!-- Replace this with a clear, honest description of what the project does.
     Write for someone who has never heard of it. One paragraph is enough. -->

[Describe what this project does and who it's for.]

## Why Does It Exist?

<!-- What problem does this solve? What was broken or missing before?
     What would someone have to do without it? -->

[Describe the problem this project solves.]

## Who Is It For?

<!-- Be specific. "Developers who need to..." is better than "anyone who..." -->

[Describe the intended audience.]

---

## Guiding Values

These values shape every decision in this project -- from architecture to naming to what gets built and what doesn't.

### Simplicity Over Cleverness

Simple code is easier to read, debug, and change. When two solutions work, choose the more obvious one. Clever code impresses no one when it breaks.

### Explicit Over Implicit

Make intent visible. Prefer clear names, obvious structure, and stated assumptions over magic, convention, and hidden behavior. Code should say what it does.

### Quality Over Speed

Shortcuts create debt. Write tests. Document decisions. Refactor when things get messy. Moving fast by cutting corners is an illusion -- you pay for it later, with interest.

### Test-First Thinking

Tests are not an afterthought. They are the first proof that something works, the specification of how it should behave, and the safety net that lets you change things confidently.

### Behavior Over Implementation

Tests should survive refactoring. Test what a thing does, not how it does it internally. If your tests break when you rename a private function, they are testing the wrong thing.

### Documentation in Git

All documentation lives in the repository as plain text. It is version controlled, reviewed in pull requests, and updated alongside the code it describes. Docs that live outside the repo rot.

### Commit to the Roadmap

Features are planned, not improvised. Every version has a clear goal. Work is done in small, complete increments -- each one shippable on its own.

---

These are not rules imposed from outside. They are conclusions drawn from building software, breaking it, fixing it, and learning from the experience. They exist to make the work better and the codebase something worth being proud of.
