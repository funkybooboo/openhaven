# Getting Started

This guide walks you through setting up the project locally from scratch.

## Prerequisites

Before you begin, make sure you have the following installed:

<!-- List what someone needs before they can run this project.
     Be specific about versions. Link to install instructions. -->

- [Tool 1] -- [what it's for and where to get it]
- [Tool 2] -- [what it's for and where to get it]

```bash
# Verify your setup
tool1 --version
tool2 --version
```

---

## Installation

**1. Clone the repository:**

```bash
git clone https://github.com/username/project-name.git
cd project-name
```

**2. Install dependencies:**

```bash
task setup
```

**3. Configure your environment:**

```bash
cp .env.example .env
```

Open `.env` and fill in the required values. Every variable is documented in `.env.example`.

**4. Run the project:**

```bash
task dev
```

---

## Configuration

All configuration is done through environment variables. Never hardcode config values in source code.

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `APP_ENV` | No | `development` | Runtime environment (`development`, `test`, `production`) |
| `LOG_LEVEL` | No | `info` | Log verbosity (`debug`, `info`, `warn`, `error`) |

<!-- Add your project's variables to this table.
     See .env.example for the full list with descriptions. -->

---

## Common Commands

| Command | Description |
|---------|-------------|
| `task setup` | Install dependencies and prepare the environment |
| `task dev` | Run the project in development mode |
| `task test` | Run all tests |
| `task validate` | Run all checks -- format, lint, type-check, test, build |
| `task build` | Build for production |
| `task --list` | Show all available commands |

---

## Project Structure

```
project-name/
|-- docs/             # Documentation
|-- plans/            # Roadmap, stories, decisions, retrospectives
|-- .github/          # CI workflow and GitHub templates
|-- .env.example      # Environment variable reference
|-- Taskfile.yml      # All development commands
|-- CHANGELOG.md      # Version history
`-- README.md
```

<!-- Add your project's source directories here once you've set them up. -->

---

## Troubleshooting

**Problem:** `task` command not found
**Solution:** Install [Task](https://taskfile.dev/installation/) -- the task runner used by this project.

**Problem:** Environment variable errors on startup
**Solution:** Make sure you've copied `.env.example` to `.env` and filled in all required values.

<!-- Add project-specific troubleshooting here as you discover common issues. -->

If you're stuck, [open an issue](https://github.com/username/project-name/issues) with your environment details and what you tried.
