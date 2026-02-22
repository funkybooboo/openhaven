# Roadmap

This document tracks what we're building, why, and in what order.

## Vision

OpenHaven becomes the Rails of personal cloud infrastructure - the obvious choice for anyone who wants to self-host with SaaS-level convenience. At its best, OpenHaven makes deploying and managing your personal cloud as simple as signing up for Gmail, while keeping you in full control of your data, infrastructure, and costs.

## Status Legend

| Symbol | Meaning |
|--------|---------|
| [x] | Complete |
| [/] | In progress |
| [ ] | Planned |
| [?] | Future / under consideration |

---

## v0.1.0 -- Documentation Foundation [/]

**Goal:** Establish vision, architecture, and planning foundation.

- [x] Repository structure and organization
- [x] Core documentation (Introduction, Architecture, Roadmap)
- [x] Architecture Decision Records (ADRs)
- [x] Technical specifications (Stack decisions)
- [ ] Onboarding flow specification
- [ ] Initial user stories
- [x] Vision document
- [x] Contributing guidelines updated

**Done when:** A contributor can read the docs and understand what OpenHaven is, why it exists, and how it will work.

---

## v0.2.0 -- Minimal Control Plane [ ]

**Goal:** Basic infrastructure provisioning working end-to-end.

- [ ] Terraform modules for single VM deployment
- [ ] Docker Compose templates for core services
- [ ] Keycloak deployment and configuration
- [ ] Vaultwarden deployment
- [ ] Gitea deployment with S3 backend
- [ ] Traefik reverse proxy with SSL automation
- [ ] Cloudflare DNS automation
- [ ] Basic CLI for deployment

**Done when:** Can provision a VM, deploy Keycloak + Vaultwarden + Gitea, and access them via HTTPS with working SSO.

---

## v0.3.0 -- File Storage & Productivity [ ]

**Goal:** Add file storage, calendar, and office capabilities.

- [ ] Nextcloud deployment with S3 backend
- [ ] Nextcloud + Keycloak SSO integration
- [ ] OnlyOffice integration with Nextcloud
- [ ] WireGuard VPN deployment
- [ ] Automated backup system to S3
- [ ] Health monitoring and status dashboard

**Done when:** Users can store files, edit documents, manage calendars/contacts, and connect via VPN.

---

## v0.4.0 -- Email & Communication [ ]

**Goal:** Self-hosted email with proper deliverability.

- [ ] Mailcow deployment
- [ ] SPF/DKIM/DMARC automation
- [ ] Reverse DNS documentation and validation
- [ ] Email deliverability testing tools
- [ ] Optional SMTP relay integration
- [ ] Spam filtering configuration

**Done when:** Users can send and receive email with good deliverability.

---

## v0.5.0 -- AI & Developer Tools [ ]

**Goal:** Self-hosted AI and enhanced developer experience.

- [ ] Ollama deployment (CPU and optional GPU)
- [ ] Open WebUI deployment
- [ ] AI API gateway for coding tools
- [ ] Model management UI
- [ ] Gitea CI/CD integration
- [ ] Development environment templates

**Done when:** Users can run local LLMs, use AI chat, and integrate AI into coding workflows.

---

## v0.6.0 -- Web Dashboard [ ]

**Goal:** Unified web interface for managing services.

- [ ] Web app framework setup
- [ ] Keycloak SSO integration
- [ ] Service launcher/portal
- [ ] File explorer (Nextcloud integration)
- [ ] Password vault access (Vaultwarden integration)
- [ ] AI chat interface
- [ ] Git repository browser
- [ ] VPN config download
- [ ] System status and monitoring

**Done when:** Users can access all services through a unified web dashboard.

---

## v0.7.0 -- Desktop Application [ ]

**Goal:** Native desktop experience.

- [ ] Tauri application framework
- [ ] SSO login flow
- [ ] File sync status and management
- [ ] System tray integration
- [ ] VPN toggle
- [ ] Notifications
- [ ] Local AI chat
- [ ] Git integration

**Done when:** Users have a native desktop app for Windows, macOS, and Linux.

---

## v0.8.0 -- Mobile Application [ ]

**Goal:** Mobile access to personal cloud.

- [ ] React Native or Flutter setup
- [ ] SSO login flow
- [ ] File browsing and upload
- [ ] Password vault access
- [ ] Calendar and contacts sync
- [ ] AI chat interface
- [ ] Push notifications
- [ ] Offline mode

**Done when:** Users can access their cloud from iOS and Android devices.

---

## v1.0.0 -- Production Ready [?]

**Goal:** Stable, secure, and ready for production use.

- [ ] Complete end-to-end testing
- [ ] Security audit and hardening
- [ ] Performance optimization
- [ ] Comprehensive documentation (user and developer)
- [ ] Migration and backup tools
- [ ] Disaster recovery procedures
- [ ] Multi-region deployment support
- [ ] Update and rollback mechanisms
- [ ] Monitoring and alerting
- [ ] Cost estimation tools

**Done when:** We're comfortable recommending OpenHaven for production use with real data.

---

## v2.0.0 -- Advanced Features [?]

**Goal:** Team collaboration and advanced capabilities.

- [ ] Multi-user/team support
- [ ] Role-based access control
- [ ] Messaging (Matrix/Synapse)
- [ ] Multi-region redundancy
- [ ] Advanced monitoring and analytics
- [ ] Plugin/extension system
- [ ] Marketplace for community extensions
- [ ] Advanced cost optimization
- [ ] Compliance tools (GDPR, etc.)

**Done when:** OpenHaven supports teams and advanced use cases.

---

## Backlog

Ideas that don't have a version yet. Promoted to a milestone when prioritized.

### High priority
<!-- - Feature idea -->

### Medium priority
<!-- - Feature idea -->

### Low priority / nice to have
<!-- - Feature idea -->

---

## How to Use This Document

- Check off items as they're completed: `- [x]`
- Update status symbols as work progresses
- Add new milestones as the project evolves
- Move backlog items into milestones when they're prioritized
- Keep acceptance criteria realistic and measurable

## Related

- [Stories](./stories/) -- user stories with acceptance criteria
- [Decisions](./decisions/) -- architecture decision records
- [CHANGELOG](../CHANGELOG.md) -- what shipped in each release
