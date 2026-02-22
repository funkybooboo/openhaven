# AI Assistant Rules

Project-specific rules for AI coding assistants working in this repository.

## Character Encoding

- Use ASCII only in source code, configuration, and documentation
- Variable names, function names, comments, commit messages, and doc prose must be ASCII
- User-supplied data (strings, file contents, database values) may be non-ASCII — handle it correctly, just don't introduce non-ASCII into the codebase itself
- No Unicode arrows (→ ← ↑ ↓), emoji, curly quotes (" "), en/em dashes (– —), or decorative symbols in code or docs
- Use plain ASCII alternatives: `->` not `→`, `--` not `—`, `"` not `"`, `...` not `…`

## Code Quality

- Write clean, maintainable, self-documenting code
- Use meaningful names; prefer clarity over cleverness
- Follow existing patterns in the codebase before introducing new ones
- Keep functions focused on a single responsibility
- Read files before editing; make surgical changes

## Testing

- Write tests alongside code — test first when practical
- Test behavior and contracts, not implementation details
- Tests must pass before finishing any task

## Documentation

- Update docs in the same change as the code they describe
- Write for the reader, not the writer — assume no prior context
- Document "why", not "what"

## Git

- Follow conventional commits: `type(scope): description`
- Keep commits small and focused
- Never commit secrets, credentials, or `.env` files

## When in Doubt

- Ask before assuming
- Prefer the simplest solution that works
- Match existing style unless asked to change it
