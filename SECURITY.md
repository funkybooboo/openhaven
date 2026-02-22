# Security Policy

## Reporting a Vulnerability

**Do not report security vulnerabilities through public GitHub issues.**

Report vulnerabilities privately using one of these methods:

- **GitHub Security Advisories** (preferred): [Report a vulnerability](https://github.com/username/project-name/security/advisories/new)
- **Email**: security@example.com

You should receive a response within 48 hours. If you don't hear back, follow up by email to confirm receipt.

## What to Include

- Type of vulnerability (e.g. SQL injection, XSS, auth bypass)
- Affected version(s) or commit range
- File paths and line numbers, if known
- Step-by-step reproduction instructions
- Proof-of-concept or exploit code, if available
- Your assessment of impact and exploitability

## Response Process

| Step | Timeline |
|------|----------|
| Acknowledgment | Within 48 hours |
| Validation and triage | Within 5 business days |
| Fix timeline communicated | Within 10 business days |
| Fix released (critical issues) | Within 30 days |

We'll coordinate public disclosure with you after the fix ships. We're happy to credit you in the release notes if you'd like.

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest  | Yes |
| Older   | No -- please upgrade |

Update this table once you have a stable release and a support policy.

## Security Best Practices

When using this project:

- Use the latest release
- Never commit secrets, API keys, or credentials to version control
- Keep dependencies up to date (`task deps:audit`)
- Review `.env.example` -- every variable is documented there
