# Architecture

This document describes OpenHaven's system architecture, deployment model, and core components.

---

## System Overview

OpenHaven is a **cloud orchestration platform** that automates the deployment and management of self-hosted services across user-owned cloud infrastructure.

```
+------------------+
|   User's Cloud   |
|    Accounts      |
|  (AWS, GCP, etc) |
+--------+---------+
         |
         | Delegated IAM
         | Permissions
         v
+------------------+       +-------------------+
|   OpenHaven      |       |   User Clients    |
|  Control Plane   +------>|  (Web/Desktop/    |
|                  |       |   Mobile Apps)    |
| - Provisioning   |       +-------------------+
| - Orchestration  |
| - Monitoring     |
+--------+---------+
         |
         | Deploys & Manages
         v
+------------------+
|  Service Stack   |
|                  |
| - Keycloak (SSO) |
| - Nextcloud      |
| - Vaultwarden    |
| - Gitea          |
| - Mailcow        |
| - WireGuard      |
| - Ollama + AI    |
+------------------+
```

---

## Core Principles

### 1. User-Owned Infrastructure

**Users own the cloud accounts, not OpenHaven.**

- Users create AWS, GCP, or other cloud provider accounts
- Users grant OpenHaven limited IAM permissions via delegation
- OpenHaven provisions infrastructure in user accounts
- Users control billing and can revoke access anytime
- If OpenHaven disappears, user infrastructure continues running

This is the foundational architectural decision. See [ADR-001](../plans/decisions/ADR-001-user-owned-infrastructure.md).

### 2. Infrastructure as Code

Everything OpenHaven deploys is generated as:

- **Terraform** - Cloud resource provisioning
- **Docker Compose** - Service orchestration
- **YAML configs** - Service configuration

All generated code is:
- Human-readable
- Version-controlled
- Exportable
- Modifiable
- Forkable

Users can inspect, customize, or take over manual management at any time.

### 3. Multi-Cloud by Design

OpenHaven doesn't lock you to a single VPS or cloud provider. Instead, it uses the best provider for each service:

- **Object Storage** - AWS S3, Backblaze B2, Cloudflare R2
- **Compute** - GCP Cloud Run, AWS EC2, Hetzner Cloud
- **DNS** - Cloudflare (automated)
- **Email Relay** - Optional (self-hosted by default)

This separation of concerns is more cost-effective and resilient.

### 4. Zero SaaS Dependencies

OpenHaven uses only:
- Raw cloud infrastructure (compute, storage, networking)
- Open source software
- Self-hosted services

No proprietary SaaS APIs. No vendor lock-in. See [ADR-002](../plans/decisions/ADR-002-no-saas-dependencies.md).

---

## Deployment Model

### Phase 1: Single VM (v0.2.0 - v0.4.0)

Initial deployment model for simplicity:

```
+------------------------+
|   User's Cloud VM      |
|  (Hetzner/DO/GCP)      |
|                        |
|  +------------------+  |
|  | Docker Compose   |  |
|  |                  |  |
|  | - Traefik        |  |
|  | - Keycloak       |  |
|  | - Vaultwarden    |  |
|  | - Gitea          |  |
|  | - Nextcloud      |  |
|  | - Mailcow        |  |
|  +------------------+  |
+------------------------+
         |
         v
+------------------------+
|   S3 Storage           |
|  (AWS/Backblaze/R2)    |
|                        |
|  - Nextcloud files     |
|  - Gitea repos         |
|  - Backups             |
+------------------------+
```

**Characteristics:**
- Single VM for all services
- Docker Compose orchestration
- Traefik reverse proxy with automatic SSL
- S3-compatible object storage for data
- Automated DNS via Cloudflare API
- Automated backups to S3

### Phase 2: Multi-Cloud (v1.0.0+)

Future deployment model for scale and cost optimization:

```
+------------------+     +------------------+     +------------------+
|  Compute Layer   |     |  Storage Layer   |     |   DNS Layer      |
|  (GCP Cloud Run) |     |  (AWS S3)        |     |  (Cloudflare)    |
|                  |     |                  |     |                  |
| - Vaultwarden    |     | - Files          |     | - Automated      |
| - Gitea          |     | - Backups        |     | - SSL certs      |
| - Open WebUI     |     | - Git objects    |     | - Records        |
+------------------+     +------------------+     +------------------+
```

---

## Core Components

### 1. Control Plane

The OpenHaven control plane is responsible for:

- **Provisioning** - Creating cloud resources via Terraform
- **Orchestration** - Deploying services via Docker Compose
- **Configuration** - Generating service configs
- **Monitoring** - Health checks and status reporting
- **Updates** - Managing service updates

**Deployment Options:**
- **Hosted** (SaaS) - OpenHaven-managed control plane (future)
- **Self-hosted** - Run control plane yourself

### 2. Provisioning Engine

Uses Terraform to provision:
- Cloud VMs
- S3 buckets
- DNS records
- SSL certificates
- Networking/firewall rules
- IAM roles and policies

**Key Features:**
- Idempotent operations
- State management
- Drift detection
- Multi-provider support

### 3. Service Stack

Opinionated selection of best-in-class open source services:

| Service | Purpose | Default Choice | Alternatives |
|---------|---------|----------------|--------------|
| Identity/SSO | Authentication | Keycloak | Authelia, Dex |
| Files/Calendar | Storage/Sync | Nextcloud | ownCloud, SeaFile |
| Email | Mail server | Mailcow | iRedMail, Modoboa |
| Passwords | Password manager | Vaultwarden | - |
| Git | Git hosting | Gitea | GitLab |
| Office | Document editing | OnlyOffice | Collabora |
| VPN | Private network | WireGuard | Tailscale |
| AI | LLM hosting | Ollama + Open WebUI | - |

See [Stack Decisions](../plans/specs/stack-decisions.md) for detailed rationale.

### 4. Client Applications

Unified interface for accessing services:

- **Web App** - Dashboard, file explorer, mail viewer, AI chat
- **Desktop App** (Tauri) - Native integration, sync status, notifications
- **Mobile App** (React Native/Flutter) - Mobile access to all services

All clients authenticate via Keycloak SSO.

---

## Data Flow

### Authentication Flow

```
1. User -> Web/Desktop/Mobile App
2. App -> Keycloak (SSO login)
3. Keycloak -> OAuth/OIDC token
4. App -> Services (with token)
5. Services -> Keycloak (validate token)
```

All services integrate with Keycloak for centralized authentication.

### File Storage Flow

```
1. User -> Nextcloud (upload file)
2. Nextcloud -> S3 (store object)
3. S3 -> Versioning + Lifecycle rules
4. Backup job -> S3 (snapshot)
```

Files never stored on VM disk - always in S3.

### Email Flow

```
Inbound:
1. External -> Mailcow (SMTP)
2. Mailcow -> Spam filter (Rspamd)
3. Mailcow -> User mailbox (Dovecot)

Outbound:
1. User -> Mailcow (SMTP)
2. Mailcow -> DKIM signing
3. Mailcow -> External (direct or relay)
```

---

## Security Model

### Principle of Least Privilege

- OpenHaven requests minimal IAM permissions
- Services run with minimal privileges
- Network isolation between services
- Secrets stored encrypted

### Secrets Management

- User credentials never stored by OpenHaven
- Service secrets generated automatically
- Encrypted at rest in user's infrastructure
- Rotatable without downtime

### Network Security

- All external traffic via HTTPS (Traefik + Let's Encrypt)
- Internal service communication via Docker network
- Firewall rules limit exposed ports
- VPN for administrative access

---

## Scalability Considerations

### v0.x - v1.0 (Single VM)

**Limits:**
- ~10-50 users
- ~1TB storage (via S3)
- Single region
- Vertical scaling only

**When to scale:**
- CPU/memory exhaustion
- Need for geographic distribution
- Regulatory requirements

### v2.0+ (Multi-Cloud)

**Capabilities:**
- Horizontal scaling
- Multi-region deployment
- Service-specific scaling
- Cost optimization per service

---

## Technology Stack

### Infrastructure
- **Terraform** - Cloud provisioning
- **Docker** - Containerization
- **Docker Compose** - Orchestration (v0.x)
- **Traefik** - Reverse proxy + SSL

### Control Plane (Future)
- **Language** - TBD (Go, Rust, or TypeScript)
- **API** - REST or GraphQL
- **Database** - PostgreSQL
- **Queue** - Redis or similar

### Client Apps (Future)
- **Web** - React or Svelte
- **Desktop** - Tauri
- **Mobile** - React Native or Flutter

---

## What This Architecture Enables

1. **User Sovereignty** - Users own everything
2. **Removability** - OpenHaven can be removed without data loss
3. **Transparency** - All infrastructure is visible and exportable
4. **Flexibility** - Services can be swapped or customized
5. **Cost Optimization** - Use cheapest provider for each service
6. **Resilience** - Multi-cloud reduces single points of failure

---

## What This Architecture Prevents

1. **Vendor Lock-In** - No proprietary formats or APIs
2. **Data Silos** - All data in user-controlled infrastructure
3. **Hidden Costs** - Users see actual cloud costs
4. **Black Boxes** - All configuration is readable
5. **Forced Upgrades** - Users control update timing

---

## Related Documentation

- [Stack Decisions](../plans/specs/stack-decisions.md) - Technology choices and rationale
- [ADR-001: User-Owned Infrastructure](../plans/decisions/ADR-001-user-owned-infrastructure.md)
- [ADR-002: No SaaS Dependencies](../plans/decisions/ADR-002-no-saas-dependencies.md)
- [Onboarding Flow](../plans/specs/onboarding-flow.md) - User experience design
- [Roadmap](../plans/roadmap.md) - Implementation timeline
