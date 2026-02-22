# Developer Guides

Feature-specific guides for contributors and maintainers.

These docs explain **how things work internally** -- architecture decisions, edge cases, integration points, and testing strategy for specific features.

## Writing Developer Guides

- Write for someone who will read or modify the source code
- Explain the *why* behind design decisions, not just the *what*
- Document edge cases and error handling explicitly
- Include the testing strategy: which test files cover this feature
- Link to relevant ADRs if a significant decision was made

## Structure

```
developer/
|-- README.md   <- you are here
`-- .gitkeep
```

Add a file here for any feature complex enough to warrant explanation beyond inline comments.
Name files after the feature: `csv-export.md`, `auth-flow.md`, `background-jobs.md`.
