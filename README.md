# OpenHaven

> Deploy your personal cloud in minutes, own it forever. Email, files, passwords, git, AI - all on infrastructure you control. 

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](./LICENSE)

---

## What Is It?

OpenHaven is a convention-driven cloud orchestrator that makes self-hosting as simple as using SaaS. Choose your domain, connect your cloud accounts, and deploy a complete personal cloud with email, file storage, password management, git hosting, and more - all running on infrastructure you own and control.

Without OpenHaven, you'd need to manually configure dozens of services, manage DNS records, set up SSL certificates, handle backups, and maintain complex infrastructure. OpenHaven automates all of this with opinionated defaults while keeping you in full control.

## Why OpenHaven?

**Multi-Cloud Cost Optimization** - OpenHaven automatically uses the cheapest and best provider for each service. Instead of locking you into one expensive VPS or SaaS platform, it intelligently distributes services across providers to minimize costs while maximizing performance.

```
┌─────────────────────────────────────────────────────────────┐
│                      Your Personal Cloud                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Compute Layer              Storage Layer                   │
│  ┌──────────────────┐       ┌──────────────────┐           │
│  │ • Email          │──────▶│ • Files          │           │
│  │ • Passwords      │       │ • Backups        │           │
│  │ • Git            │       │ • Attachments    │           │
│  │ • AI             │       └──────────────────┘           │
│  │ • Calendar       │                                       │
│  └──────────────────┘       DNS Layer                      │
│                              ┌──────────────────┐           │
│                              │ • Automation     │           │
│                              │ • SSL Certs      │           │
│                              └──────────────────┘           │
│                                                              │
│  Each layer uses the cheapest, best provider available      │
│  Total: $5-10/month  |  SaaS Equivalent: $40-50/month      │
└─────────────────────────────────────────────────────────────┘
```

**Convention Over Configuration** - Like Ruby on Rails for cloud infrastructure. Opinionated defaults automatically select the lowest-cost providers and best open-source software. Nothing is locked - customize anything.

**True Sovereignty** - You own the cloud accounts. You own the data. You control the billing. OpenHaven is just a convenience layer you can remove anytime.

**Zero Vendor Lock-In** - Everything is open source. All infrastructure is exportable as Terraform. No proprietary APIs or SaaS dependencies.

## Cost Comparison

| What You Get | SaaS Approach | OpenHaven |
|--------------|---------------|-----------|
| Email + Calendar | $5-10/month | Included |
| File Storage (100GB) | $2-5/month | ~$0.50/month |
| Password Manager | $1-3/month | Included |
| Git Hosting | $4-7/month | Included |
| VPN | $5-10/month | Included |
| AI Chat | $20/month | Included |
| Office Suite | $7-10/month | Included |
| **Total** | **$44-65/month** | **$5-10/month** |

**Savings: 75-85% ($400-600/year)**

Plus you own everything - no vendor lock-in, no data mining, full control.

See [Stack Decisions](./plans/specs/stack-decisions.md) for detailed provider and software choices.

## Current Status

**v0.1.0 - Documentation Phase** (In Progress)

OpenHaven is in early development. We're currently establishing the vision, architecture, and roadmap. See [Roadmap](./plans/roadmap.md) for details.

## Getting Started

OpenHaven is not yet ready for deployment. To contribute to the vision and planning:

```bash
git clone https://github.com/funkybooboo/openhaven.git
cd openhaven
# Read the documentation
cat docs/01-introduction.md
cat plans/roadmap.md
```

See [Contributing](./CONTRIBUTING.md) to get involved in shaping OpenHaven.

## Documentation

| | |
|---|---|
| [Introduction](./docs/01-introduction.md) | What this project is and the values behind it |
| [Getting Started](./docs/02-getting-started.md) | Setup, configuration, and your first run |
| [Architecture](./docs/03-architecture.md) | How the project is structured and why |
| [Code Standards](./docs/04-code-standards.md) | Naming, style, and enforcement |
| [Testing](./docs/05-testing.md) | Philosophy, strategy, and how to run tests |
| [Contributing](./docs/06-contributing.md) | How to get involved |
| [Git Workflow](./docs/07-git-workflow.md) | Branching, commits, and pull requests |
| [Design Patterns](./docs/08-design-patterns.md) | Patterns used in this project and when to apply them |
| [Feature Development](./docs/09-feature-development-loop.md) | End-to-end process from idea to production |
| [Documentation Standards](./docs/10-documentation-standards.md) | How to write and maintain docs |

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md) to get started.

## License

[GPL-3.0](./LICENSE)
