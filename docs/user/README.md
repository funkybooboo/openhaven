# User Documentation

Guides for people who use this project.

These docs explain **what you can do** and **how to do it** -- not how it works internally.

## Writing User Docs

- Write for someone who has never seen the source code
- Use plain language -- no jargon, no implementation details
- Lead with the task, not the feature: "How to export your data" not "The export system"
- Include examples for every non-obvious step
- Add a Troubleshooting section for anything that commonly goes wrong

## Structure

```
user/
|-- README.md          <- you are here
|-- getting-started.md <- first-run guide for end users (fill in placeholders)
`-- features/          <- one file per major feature
    `-- .gitkeep
```

Add a file here whenever a feature ships that users need to know about.
