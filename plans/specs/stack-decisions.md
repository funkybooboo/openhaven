# OpenHaven Technology Stack Decisions

This document details the opinionated technology choices for each service category in OpenHaven, including rationale, alternatives, and override mechanisms.

**Last Updated:** 2026-02-22  
**Status:** Planning (v0.1.0)

---

## Philosophy

OpenHaven makes opinionated choices to eliminate decision paralysis. For each service category, we select:

1. **Default (Tier 1)** - Best balance of cost, reliability, and openness
2. **Alternatives (Tier 2)** - Supported but not default
3. **Future (Tier 3)** - Planned for later versions

Users can override any default, but don't have to make decisions to get started.

---

## Selection Criteria

For each service, we evaluate:

- **Cost** - Total cost of ownership (compute + storage + bandwidth)
- **Reliability** - Production-readiness and stability
- **Openness** - Open source license and community
- **Ease of Deployment** - Docker support, configuration complexity
- **Integration** - Works with other services (especially Keycloak SSO)
- **Maintenance** - Update frequency, security track record
- **Documentation** - Quality of docs and community support

---

## Infrastructure Layer

### Compute

| Tier | Provider | Use Case | Why |
|------|----------|----------|-----|
| 1 | **Hetzner Cloud** | Single VM deployment | Cheapest, excellent performance, EU-based |
| 2 | DigitalOcean | Alternative VM | Good docs, US-based |
| 2 | GCP Cloud Run | Serverless (future) | Auto-scaling, pay-per-use |
| 2 | AWS EC2 | Enterprise users | Wide availability, integrations |

**Default for v0.2-v0.4:** Hetzner Cloud CX21 (2 vCPU, 4GB RAM, ~$5/month)

**Rationale:**
- Hetzner offers best price/performance
- Simple pricing, no hidden costs
- Excellent network performance
- EU data residency (GDPR-friendly)
- Can upgrade to larger instances easily

**Override:** Users can specify any cloud provider in config

### Object Storage

| Tier | Provider | Use Case | Why |
|------|----------|----------|-----|
| 1 | **Backblaze B2** | Primary storage | Cheapest S3-compatible, no egress fees |
| 2 | AWS S3 | Enterprise/integration | Industry standard, wide compatibility |
| 2 | Cloudflare R2 | Zero egress costs | Good for high-bandwidth use cases |
| 2 | DigitalOcean Spaces | Simplicity | Easy to use, predictable pricing |

**Default for v0.2+:** Backblaze B2

**Rationale:**
- $5/TB/month storage (cheapest)
- Free egress up to 3x storage
- S3-compatible API
- Excellent reliability
- No minimum storage fees

**Override:** Users can specify any S3-compatible provider

### DNS

| Tier | Provider | Why |
|------|----------|-----|
| 1 | **Cloudflare** | Free tier, excellent API, DDoS protection |
| 2 | Route53 | AWS integration |
| 2 | DigitalOcean DNS | Simplicity |

**Default:** Cloudflare DNS (free tier)

**Rationale:**
- Free for unlimited domains
- Excellent API for automation
- Built-in DDoS protection
- Fast global anycast network
- Optional proxy features

---

## Identity & Authentication

| Tier | Software | Why |
|------|----------|-----|
| 1 | **Keycloak** | Enterprise-grade, OAuth/OIDC, extensive integrations |
| 2 | Authelia | Lightweight SSO |
| 3 | Dex | Kubernetes-native (future) |

**Default:** Keycloak

**Rationale:**
- Industry-standard OAuth/OIDC provider
- Extensive integration support
- User federation capabilities
- Multi-factor authentication
- Social login support
- Active development and community
- Used by enterprises (proven at scale)

**Integrations:**
- Nextcloud (OIDC)
- Gitea (OAuth)
- Vaultwarden (SSO via proxy)
- Custom web/desktop/mobile apps

**Resources:**
- CPU: 0.5-1 vCPU
- Memory: 512MB-1GB
- Storage: Minimal (PostgreSQL backend)

---

## File Storage, Calendar & Contacts

| Tier | Software | Storage Backend | Why |
|------|----------|-----------------|-----|
| 1 | **Nextcloud** | S3 | Feature-rich, mature, extensive app ecosystem |
| 2 | ownCloud | S3 | Similar to Nextcloud, enterprise focus |
| 3 | SeaFile | Local | Lighter, fewer features |

**Default:** Nextcloud with S3 primary storage

**Rationale:**
- Comprehensive feature set (files, calendar, contacts, notes)
- WebDAV/CalDAV/CardDAV support
- OnlyOffice integration
- Mobile apps (iOS/Android)
- Desktop sync clients
- S3 as primary storage (not just external storage)
- Active development
- Large plugin ecosystem

**Features Enabled by Default:**
- Files (WebDAV)
- Calendar (CalDAV)
- Contacts (CardDAV)
- Notes
- OnlyOffice integration
- Keycloak SSO

**Resources:**
- CPU: 1-2 vCPU
- Memory: 1-2GB
- Storage: S3 (unlimited, pay-per-use)

---

## Email

| Tier | Software | Components | Why |
|------|----------|------------|-----|
| 1 | **Mailcow** | Postfix, Dovecot, Rspamd, SOGo | Most complete, Docker-native, excellent UI |
| 2 | iRedMail | Postfix, Dovecot, various | Comprehensive, traditional install |
| 3 | Modoboa | Postfix, Dovecot | Python-based, modern UI |

**Default:** Mailcow

**Rationale:**
- Complete mail server suite in Docker
- Excellent web UI for administration
- Built-in webmail (SOGo)
- Spam filtering (Rspamd)
- DKIM/SPF/DMARC automation
- Active development
- Good documentation
- Community support

**Components:**
- **Postfix** - SMTP server
- **Dovecot** - IMAP/POP3 server
- **Rspamd** - Spam filtering
- **SOGo** - Webmail and groupware
- **ClamAV** - Antivirus
- **Solr** - Full-text search

**Deliverability Strategy:**
- Automated SPF/DKIM/DMARC setup
- Reverse DNS validation
- IP reputation monitoring
- Deliverability testing tools
- Optional SMTP relay configuration (user-managed)

**Resources:**
- CPU: 2-4 vCPU
- Memory: 4-6GB
- Storage: Minimal (emails in mailboxes)

**Challenges:**
- Email deliverability requires proper DNS setup
- IP reputation takes time to build
- May need SMTP relay for critical emails initially

---

## Password Manager

| Tier | Software | Why |
|------|----------|-----|
| 1 | **Vaultwarden** | Bitwarden-compatible, lightweight, feature-complete |

**Default:** Vaultwarden (only option)

**Rationale:**
- Unofficial Bitwarden server implementation
- Compatible with official Bitwarden clients
- Extremely lightweight (10-20MB RAM)
- All Bitwarden features
- Active development
- Excellent security track record
- No viable self-hosted alternatives

**Features:**
- Password storage
- Secure notes
- TOTP generation
- Password sharing
- Organizations
- Browser extensions
- Mobile apps
- Desktop apps

**Resources:**
- CPU: 0.1 vCPU
- Memory: 20-50MB
- Storage: Minimal (encrypted vault)

---

## Git Hosting

| Tier | Software | Why |
|------|----------|-----|
| 1 | **Gitea** | Lightweight, fast, S3 support |
| 2 | GitLab CE | Feature-rich, CI/CD, heavier |
| 3 | Gogs | Lighter than Gitea, fewer features |

**Default:** Gitea

**Rationale:**
- Lightweight (written in Go)
- Fast and responsive
- S3 backend for LFS and attachments
- GitHub-like UI
- Pull requests, issues, wikis
- OAuth integration (Keycloak)
- CI/CD support (Gitea Actions)
- Low resource usage

**Features:**
- Git repository hosting
- Pull requests and code review
- Issue tracking
- Wiki
- Releases
- Webhooks
- Git LFS (S3-backed)
- CI/CD (Gitea Actions)

**Resources:**
- CPU: 0.5-1 vCPU
- Memory: 256-512MB
- Storage: S3 for repos and LFS

---

## VPN

| Tier | Software | Why |
|------|----------|-----|
| 1 | **WireGuard** | Fast, modern, simple, secure |
| 2 | Tailscale (self-hosted) | Mesh networking, NAT traversal |
| 3 | OpenVPN | Traditional, widely supported |

**Default:** WireGuard

**Rationale:**
- Modern, secure protocol
- Extremely fast
- Simple configuration
- Built into Linux kernel
- Low overhead
- Easy client setup (QR codes)
- Cross-platform clients

**Features:**
- Site-to-site VPN
- Road warrior (client-to-site)
- QR code config generation
- Automatic key management
- Split tunneling support

**Resources:**
- CPU: 0.1-0.5 vCPU
- Memory: 50-100MB
- Bandwidth: Minimal overhead

---

## Online Office

| Tier | Software | Integration | Why |
|------|----------|-------------|-----|
| 1 | **OnlyOffice** | Nextcloud | Best Nextcloud integration, MS Office compatible |
| 2 | Collabora Online | Nextcloud | LibreOffice-based |
| 3 | Etherpad | Standalone | Simple collaborative editing |

**Default:** OnlyOffice (integrated with Nextcloud)

**Rationale:**
- Excellent Microsoft Office compatibility
- Real-time collaboration
- Native Nextcloud integration
- Modern UI
- Mobile editing support
- Supports DOCX, XLSX, PPTX natively

**Features:**
- Document editing (Word-compatible)
- Spreadsheet editing (Excel-compatible)
- Presentation editing (PowerPoint-compatible)
- Real-time collaboration
- Comments and track changes
- Mobile apps

**Resources:**
- CPU: 1-2 vCPU
- Memory: 1-2GB
- Storage: Minimal (documents in Nextcloud)

---

## AI & LLM Hosting

| Tier | Software | Backend | Why |
|------|----------|---------|-----|
| 1 | **Ollama + Open WebUI** | Local models | Self-hosted, privacy-first, hardware-agnostic |
| 2 | LocalAI | Local models | OpenAI-compatible API |
| 3 | Text Generation WebUI | Local models | Advanced features, heavier |

**Default:** Ollama + Open WebUI

**Rationale:**
- Ollama: Easy model management, fast inference
- Open WebUI: ChatGPT-like interface
- Runs on CPU or GPU
- Privacy-first (no data leaves infrastructure)
- OpenAI-compatible API
- Model marketplace
- Supports coding assistants (Continue, Cody, etc.)

**Supported Models:**
- Llama 2/3
- Mistral
- CodeLlama
- Phi
- Many others via Ollama library

**Features:**
- Chat interface
- API for coding tools
- Model management
- Conversation history
- Multi-user support
- Keycloak SSO integration

**Resources (CPU mode):**
- CPU: 4-8 vCPU
- Memory: 8-16GB
- Storage: 10-50GB (models)

**Resources (GPU mode):**
- GPU: NVIDIA with 8GB+ VRAM
- CPU: 2-4 vCPU
- Memory: 8-16GB
- Storage: 10-50GB (models)

---

## Reverse Proxy & SSL

| Tier | Software | Why |
|------|----------|-----|
| 1 | **Traefik** | Docker-native, automatic SSL, service discovery |
| 2 | Caddy | Simple config, automatic HTTPS |
| 3 | Nginx Proxy Manager | Traditional, GUI |

**Default:** Traefik

**Rationale:**
- Native Docker integration
- Automatic service discovery
- Let's Encrypt integration (automatic SSL)
- Dynamic configuration
- HTTP/2 and HTTP/3 support
- Middleware support (auth, rate limiting, etc.)
- Excellent documentation

**Features:**
- Automatic SSL certificates (Let's Encrypt)
- HTTP to HTTPS redirect
- Service discovery via Docker labels
- Load balancing
- Rate limiting
- Basic auth middleware
- Access logs

**Resources:**
- CPU: 0.1-0.5 vCPU
- Memory: 50-100MB

---

## Database

| Tier | Software | Use Case | Why |
|------|----------|----------|-----|
| 1 | **PostgreSQL** | Primary database | Industry standard, reliable, feature-rich |
| 2 | MariaDB | Alternative | MySQL-compatible |
| 3 | SQLite | Lightweight services | Embedded, no server |

**Default:** PostgreSQL

**Rationale:**
- Industry-standard relational database
- Excellent performance
- ACID compliance
- JSON support
- Full-text search
- Extensive ecosystem
- Used by Keycloak, Nextcloud, Gitea, etc.

**Resources:**
- CPU: 0.5-1 vCPU
- Memory: 512MB-1GB
- Storage: Varies by service

---

## Monitoring & Observability

| Tier | Software | Purpose | Why |
|------|----------|---------|-----|
| 1 | **Uptime Kuma** | Status monitoring | Simple, beautiful, self-hosted |
| 2 | Prometheus + Grafana | Metrics | Industry standard |
| 3 | Netdata | Real-time monitoring | Comprehensive, heavy |

**Default (v0.3+):** Uptime Kuma

**Rationale:**
- Simple status page
- Beautiful UI
- Multi-protocol monitoring (HTTP, TCP, ping, etc.)
- Notifications (email, Slack, etc.)
- Lightweight
- Easy to configure

**Future (v1.0+):** Add Prometheus + Grafana for detailed metrics

**Resources:**
- CPU: 0.1 vCPU
- Memory: 100-200MB

---

## Backup

| Tier | Software | Destination | Why |
|------|----------|-------------|-----|
| 1 | **Restic** | S3 | Encrypted, deduplicated, efficient |
| 2 | Borg | S3 | Similar to Restic |
| 3 | Duplicati | S3 | GUI, cross-platform |

**Default:** Restic to S3

**Rationale:**
- Encrypted backups
- Deduplication (saves space)
- Incremental backups
- S3-compatible backends
- Fast and efficient
- Easy to restore
- Well-tested

**Backup Strategy:**
- Daily incremental backups
- Weekly full backups
- 30-day retention
- Encrypted at rest
- Stored in S3 (separate bucket)

**What's Backed Up:**
- PostgreSQL databases
- Docker volumes
- Configuration files
- Keycloak data
- Vaultwarden vault
- Gitea repositories (if not in S3)

**What's NOT Backed Up:**
- Nextcloud files (already in S3)
- Gitea LFS (already in S3)
- Docker images (reproducible)

---

## Messaging (Future - v2.0+)

| Tier | Software | Protocol | Why |
|------|----------|----------|-----|
| 1 | **Matrix (Synapse)** | Matrix | Federated, open, feature-rich |
| 2 | Rocket.Chat | Proprietary | Slack-like, easier |
| 3 | Mattermost | Proprietary | Team collaboration |

**Default (future):** Matrix + Synapse

**Rationale:**
- Open protocol
- Federation support
- End-to-end encryption
- Bridges to other platforms
- Active development
- Mobile apps

**Not included in v1.0** due to resource requirements and complexity.

---

## Override Mechanism

Users can override any default choice via configuration:

```yaml
# openhaven.yml
services:
  identity:
    provider: keycloak  # default
    # override: authelia
  
  files:
    provider: nextcloud  # default
    storage: backblaze-b2  # default
    # storage: aws-s3
  
  email:
    provider: mailcow  # default
    # provider: iredmail
    relay:
      enabled: false  # default
      # provider: smtp2go
  
  git:
    provider: gitea  # default
    # provider: gitlab
  
  ai:
    provider: ollama  # default
    mode: cpu  # default
    # mode: gpu
    # models:
    #   - llama2
    #   - codellama
```

---

## Version Roadmap

### v0.2.0 - Core Services
- Keycloak
- Vaultwarden
- Gitea
- PostgreSQL
- Traefik

### v0.3.0 - Productivity
- Nextcloud
- OnlyOffice
- WireGuard
- Uptime Kuma
- Restic backups

### v0.4.0 - Email
- Mailcow

### v0.5.0 - AI
- Ollama
- Open WebUI

### v1.0.0 - Polish
- All services production-ready
- Comprehensive monitoring
- Automated updates

### v2.0.0 - Advanced
- Matrix messaging
- Multi-region support
- Team features

---

## Cost Estimates

### Minimal Setup (v0.2)
- Hetzner CX21: $5/month
- Backblaze B2 (10GB): $0.05/month
- Cloudflare DNS: Free
- **Total: ~$5/month**

### Standard Setup (v0.3)
- Hetzner CX31: $10/month
- Backblaze B2 (100GB): $0.50/month
- Cloudflare DNS: Free
- **Total: ~$11/month**

### Full Setup (v0.5)
- Hetzner CX41: $20/month
- Backblaze B2 (500GB): $2.50/month
- Cloudflare DNS: Free
- **Total: ~$23/month**

### With GPU (AI)
- Hetzner CCX33 (GPU): $100/month
- Backblaze B2 (500GB): $2.50/month
- Cloudflare DNS: Free
- **Total: ~$103/month**

**Compare to SaaS:**
- Proton Unlimited: $13/month (email + drive + VPN + calendar)
- GitHub Pro: $4/month
- Bitwarden Premium: $10/year
- ChatGPT Plus: $20/month
- **Total: ~$38/month** (without office suite, git hosting, etc.)

**OpenHaven savings: 40-70% vs. equivalent SaaS**

---

## Related Documentation

- [Architecture](../../docs/03-architecture.md)
- [ADR-002: No SaaS Dependencies](../decisions/ADR-002-no-saas-dependencies.md)
- [Roadmap](../roadmap.md)
