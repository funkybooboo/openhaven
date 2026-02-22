# OpenHaven Vision

This document captures the long-term vision, strategic positioning, and core philosophy of OpenHaven.

---

## The Problem

Today, individuals and small teams face an impossible choice:

### Option A: SaaS Platforms (Proton, Google, Microsoft, etc.)

**Pros:**
- Extremely convenient
- Professional reliability
- No technical knowledge required

**Cons:**
- Expensive recurring costs
- No control over data or infrastructure
- Vendor lock-in
- Privacy concerns
- Limited customization

### Option B: Self-Hosting

**Pros:**
- Full control over data and infrastructure
- Potentially much cheaper
- Privacy and sovereignty
- Customizable

**Cons:**
- Steep learning curve
- Fragmented ecosystem (dozens of tools)
- Complex setup (DNS, SSL, email deliverability, backups)
- Ongoing maintenance burden
- Decision paralysis (which mail server? which file storage?)
- Email deliverability challenges
- Security update management

**The gap:** There is no middle ground that provides SaaS-level convenience with self-hosted sovereignty.

---

## The Solution: OpenHaven

OpenHaven is a **convention-driven cloud orchestrator** that makes self-hosting as simple as using SaaS, while keeping users in full control.

### Core Thesis

> Self-hosting should be as simple as signing up for Gmail, but with the user owning the infrastructure.

We achieve this through:

1. **Convention Over Configuration** - Opinionated defaults eliminate decision paralysis
2. **User-Owned Infrastructure** - Users own cloud accounts, OpenHaven just orchestrates
3. **Zero Vendor Lock-In** - Everything is open source, exportable, and removable
4. **Multi-Cloud by Design** - Use the best provider for each service
5. **Infrastructure as Code** - All configuration is transparent and modifiable

---

## What Makes OpenHaven Different

OpenHaven is not:
- Another homelab dashboard
- Another Docker GUI
- Another app marketplace
- Another managed hosting service

OpenHaven is:
- **A cloud orchestration platform** that automates infrastructure provisioning
- **A convention-driven system** that makes opinionated choices for you
- **A sovereignty-first tool** that prioritizes user control over our convenience
- **A removable layer** that can be taken away without breaking your cloud

### Key Differentiators

#### 1. Rails Philosophy for Cloud Infrastructure

Like Ruby on Rails transformed web development by providing sensible defaults and convention over configuration, OpenHaven does the same for personal cloud infrastructure.

You don't choose between:
- 12 different mail servers
- 8 file storage solutions
- 15 password managers
- 20 deployment strategies

OpenHaven makes the best choice based on cost, reliability, and openness. You can override any default, but you don't have to make decisions to get started.

#### 2. User-Owned Infrastructure Model

**Critical difference:** Users create and own the cloud accounts (AWS, GCP, etc.). OpenHaven uses delegated IAM permissions to provision infrastructure in user accounts.

This means:
- Users control billing
- Users own the data
- Users can revoke OpenHaven access anytime
- If OpenHaven disappears, infrastructure keeps running
- No vendor lock-in to OpenHaven

This is fundamentally different from managed services where the provider owns the infrastructure.

#### 3. Multi-Cloud Architecture

Instead of forcing everything onto one VPS, OpenHaven uses the best provider for each service:

- **Object Storage** - Cheapest S3-compatible (AWS S3, Backblaze B2, Cloudflare R2)
- **Compute** - Right-sized VMs or serverless (GCP Cloud Run, AWS EC2, Hetzner)
- **DNS** - Automated (Cloudflare)
- **Email Relay** - Optional (self-hosted by default)

This separation is more cost-effective and resilient than single-provider solutions.

#### 4. Zero SaaS Dependencies

OpenHaven uses only:
- Raw cloud infrastructure (compute, storage, networking)
- Open source software
- Self-hosted services and models

No SendGrid, no Auth0, no OpenAI API, no proprietary services. True sovereignty.

#### 5. Radical Transparency

Everything OpenHaven generates is:
- Human-readable (Terraform, Docker Compose, YAML)
- Version-controlled
- Exportable
- Modifiable
- Forkable

Users can inspect every configuration, modify anything, or take over manual management at any time.

---

## Target Audience

### Primary Users

1. **Privacy-Conscious Individuals**
   - Want to own their data
   - Willing to pay cloud costs but not SaaS premiums
   - Don't want to become sysadmins

2. **Developers and Technical Users**
   - Understand the value of self-hosting
   - Tired of operational burden
   - Want infrastructure as code
   - Need AI and developer tools

3. **Small Teams and Families**
   - Want shared infrastructure
   - Need cost-effective solution
   - Value sovereignty over convenience

### User Characteristics

- Comfortable with basic technical concepts (domains, cloud providers, SSH)
- Not necessarily DevOps experts
- Value privacy and control
- Willing to invest time in initial setup for long-term benefits
- Prefer open source and standard protocols

---

## Strategic Positioning

### We Are NOT

- **Enterprise platform** - Built for individuals and small teams, not corporations
- **Homelab distro** - Designed for cloud deployment, not home servers
- **Managed service** - Users operate their own infrastructure
- **Zero-config magic** - Opinionated, but not hiding complexity

### We ARE

- **Cloud orchestrator** - Automates multi-cloud infrastructure
- **Convention-driven** - Sensible defaults, minimal decisions
- **Sovereignty-first** - User control is non-negotiable
- **Developer-friendly** - Infrastructure as code, Git-based, transparent

### Competitive Landscape

| Solution | Convenience | Sovereignty | Cost | Lock-In |
|----------|-------------|-------------|------|---------|
| **Proton/Google** | ⭐⭐⭐⭐⭐ | ❌ | 💰💰💰 | High |
| **Cloudron** | ⭐⭐⭐⭐ | ⭐⭐⭐ | 💰💰 | Medium |
| **YunoHost** | ⭐⭐⭐ | ⭐⭐⭐⭐ | 💰 | Low |
| **DIY Self-Hosting** | ⭐ | ⭐⭐⭐⭐⭐ | 💰 | None |
| **OpenHaven** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | 💰 | None |

**OpenHaven's sweet spot:** High convenience + full sovereignty + low cost + zero lock-in.

---

## Business Model (Future)

Inspired by n8n, Supabase, and other open-core companies:

### Free (Open Source)

- Self-hosted control plane
- All Terraform modules and templates
- Complete documentation
- Community support

### Paid (Optional SaaS)

- Hosted control plane (convenience)
- Managed updates and monitoring
- Email deliverability assistance
- Priority support
- Advanced features (multi-region, teams, etc.)

**Key principle:** The paid tier is pure convenience. Everything can be self-hosted.

---

## Hard Problems We Must Solve

### 1. Email Deliverability

Self-hosted email is notoriously difficult due to:
- SPF/DKIM/DMARC configuration
- IP reputation management
- Spam filtering
- Reverse DNS requirements

**Our approach:**
- Automate SPF/DKIM/DMARC setup
- Provide deliverability testing tools
- Document IP warmup procedures
- Offer optional SMTP relay integration
- Set realistic expectations

### 2. Security Updates

Users must keep services updated without breaking things.

**Our approach:**
- Automated update notifications
- Tested update procedures
- Rollback mechanisms
- Security advisory monitoring

### 3. DNS Automation

Cross-provider DNS management is complex.

**Our approach:**
- Start with Cloudflare (excellent API)
- Abstract DNS operations
- Support other providers later

### 4. Complexity Management

Balancing simplicity with power is hard.

**Our approach:**
- Opinionated defaults for 80% use case
- Advanced mode for customization
- Progressive disclosure of complexity
- Clear documentation of tradeoffs

---

## Success Metrics

### v1.0 Success Criteria

- **Time to Deploy:** < 30 minutes from start to working cloud
- **User Satisfaction:** Users feel in control, not overwhelmed
- **Cost Savings:** 50-70% cheaper than equivalent SaaS
- **Reliability:** 99.5%+ uptime for core services
- **Community:** Active contributors and users

### Long-Term Vision

- **Adoption:** 10,000+ active deployments
- **Ecosystem:** Community plugins and extensions
- **Recognition:** Known as "the Rails of personal cloud"
- **Sustainability:** Viable business model supporting development

---

## Guiding Principles

These principles guide every decision:

1. **User Sovereignty First** - Users must own their infrastructure and data
2. **Convention Over Configuration** - Opinionated defaults, minimal decisions
3. **Radical Transparency** - All infrastructure visible, exportable, modifiable
4. **No Vendor Lock-In** - Users can leave anytime without data loss
5. **Open Source Foundation** - Core platform is and remains open source
6. **Cost Optimization** - Use cheapest provider for each service
7. **Security by Default** - Secure configurations out of the box
8. **Progressive Disclosure** - Simple by default, powerful when needed

---

## What We Will NOT Do

To maintain focus and integrity:

- **Not enterprise-focused** - We serve individuals and small teams
- **Not a managed service** - Users operate their own infrastructure
- **Not proprietary** - No closed-source core components
- **Not SaaS-dependent** - No required external services
- **Not lock-in** - Users can always export and leave
- **Not feature-bloated** - Focus on core use cases

---

## Roadmap Philosophy

### Incremental Value Delivery

Each version should deliver complete, usable value:
- v0.2: Deploy basic services (Keycloak, Vaultwarden, Gitea)
- v0.3: Add productivity (Nextcloud, OnlyOffice, VPN)
- v0.4: Add email (Mailcow)
- v0.5: Add AI (Ollama, Open WebUI)

### Prove Core Thesis Early

Focus v0.2-v0.4 on proving:
- Onboarding simplicity
- User-owned infrastructure model
- Removability and transparency

### Expand Carefully

Don't add features that compromise core principles:
- Each feature must respect user sovereignty
- Each feature must be removable
- Each feature must be open source

---

## Long-Term Vision (3-5 Years)

OpenHaven becomes:

1. **The Default Choice** for privacy-conscious self-hosters
2. **A Thriving Ecosystem** with community plugins and extensions
3. **A Sustainable Business** supporting full-time development
4. **A Standard** that other projects build on and integrate with
5. **A Movement** promoting digital sovereignty and user control

---

## Inspiration

OpenHaven draws inspiration from:

- **Ruby on Rails** - Convention over configuration, opinionated defaults
- **Bandcamp** - User-first, independent ownership
- **n8n** - Open source core, optional SaaS
- **Supabase** - Developer experience, transparency
- **Tailscale** - Complex tech made simple
- **Cloudflare** - Best-in-class developer experience

---

## Call to Action

If you believe in:
- Digital sovereignty
- User control over data
- Open source software
- Convention over configuration
- Making self-hosting accessible

Then OpenHaven needs you. Join us in building the future of personal cloud infrastructure.

---

**Last Updated:** 2026-02-22  
**Status:** Vision document - guiding development of v0.1.0+
