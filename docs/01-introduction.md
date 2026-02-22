# Introduction

Welcome. This document explains what this project is, why it exists, and the values that guide how it's built.

---

## What Is This Project?

OpenHaven is a cloud orchestration platform that makes self-hosting your personal cloud as simple as using SaaS. It automates the deployment and management of email, file storage, calendars, password management, git hosting, VPN, AI services, and more - all running on cloud infrastructure you own and control.

Think of it as "Ruby on Rails for personal cloud infrastructure" - opinionated defaults get you running in minutes, but nothing is locked if you want to customize.

## Why Does It Exist?

Today, you must choose between convenience and sovereignty:

**SaaS (Proton, Google, etc.)** is convenient but expensive, locks you into vendors, and gives you no control over your data or infrastructure.

**Self-hosting** gives you full control and can be cheaper, but requires deep technical knowledge, managing dozens of fragmented tools, configuring complex services like email servers, handling DNS automation, SSL certificates, backups, monitoring, and security updates.

OpenHaven solves this by providing a **convention-driven orchestration layer** that automates all the complexity while keeping you in full control. You own the cloud accounts, you own the data, you control the billing - OpenHaven just makes it work.

## Who Is It For?

OpenHaven is built for:

- **Privacy-conscious individuals** who want to own their data but don't want to become sysadmins
- **Developers and technical users** who understand the value of self-hosting but are tired of the operational burden
- **Small teams and families** who want shared infrastructure without SaaS costs or vendor lock-in
- **Anyone** who believes in digital sovereignty and wants SaaS-level convenience without giving up control

You should be comfortable with basic technical concepts (domains, cloud providers, SSH) but you don't need to be a DevOps expert.

---

## What Makes OpenHaven Different?

OpenHaven is not another homelab dashboard, app marketplace, or Docker GUI. Here's what sets it apart:

### 1. Convention Over Configuration

Like Ruby on Rails transformed web development, OpenHaven applies the same philosophy to cloud infrastructure. You don't choose between 12 mail servers, 8 file storage solutions, and 15 password managers. OpenHaven makes the best choice for you based on cost, reliability, and openness. You can override any default, but you don't have to make decisions to get started.

### 2. User-Owned Infrastructure

You create the cloud accounts (AWS, GCP, etc.). You own the billing. You control the data. OpenHaven uses delegated IAM permissions to provision infrastructure in YOUR accounts - it never owns your infrastructure. If OpenHaven disappears tomorrow, your S3 buckets, VMs, and services keep running.

### 3. Zero Vendor Lock-In

Everything OpenHaven deploys is:
- Open source software
- Exportable as Terraform/Docker Compose
- Removable without breaking your cloud
- Based on standard protocols (WebDAV, CalDAV, IMAP, etc.)

OpenHaven is a convenience layer, not a cage.

### 4. Multi-Cloud by Design

Instead of forcing everything onto one VPS, OpenHaven uses the best provider for each service:
- Cheapest object storage (S3/Backblaze/R2)
- Right-sized compute (Cloud Run/EC2/Hetzner)
- Automated DNS (Cloudflare)
- Separate storage from compute

This is architecturally smarter and more cost-effective.

### 5. No SaaS Dependencies

OpenHaven uses only:
- Raw cloud infrastructure (compute, storage, networking)
- Open source software
- Self-hosted models (for AI features)

No SendGrid, no Auth0, no proprietary APIs. True sovereignty.

### 6. Infrastructure as Code

Everything generated is human-readable:
- Terraform configurations
- Docker Compose files
- YAML definitions

You can inspect, modify, fork, or take over manually at any time. Nothing is hidden.

---

## What OpenHaven Is NOT

To set clear expectations:

- **Not an enterprise platform** - Built for individuals, families, and small teams
- **Not a homelab distro** - Designed for cloud deployment, not home servers
- **Not a Docker GUI** - Higher-level orchestration, not just container management
- **Not a managed service** - You own and operate the infrastructure (though we may offer optional hosted control plane in the future)
- **Not production-ready yet** - Currently in early development (v0.1.0)

---

## Guiding Values

These values shape every decision in this project -- from architecture to naming to what gets built and what doesn't.

### User Sovereignty First

Users must own their infrastructure and data. Every architectural decision prioritizes user control over our convenience. We build tools that empower, not platforms that lock in.

### Convention Over Configuration

Opinionated defaults eliminate decision paralysis. The best choice should be automatic, but never mandatory. Users get running fast, then customize if they want.

### Radical Transparency

All infrastructure is visible, exportable, and modifiable. No hidden abstractions, no proprietary formats, no magic. If we generate it, you can read it, change it, and own it.

### Simplicity Over Cleverness

Simple code is easier to read, debug, and change. When two solutions work, choose the more obvious one. Clever code impresses no one when it breaks.

### Explicit Over Implicit

Make intent visible. Prefer clear names, obvious structure, and stated assumptions over magic and hidden behavior. Code should say what it does.

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
