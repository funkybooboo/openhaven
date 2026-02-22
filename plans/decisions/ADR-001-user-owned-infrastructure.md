# ADR-001: User-Owned Infrastructure Model

**Status:** Accepted  
**Date:** 2026-02-22  
**Deciders:** Core Team  
**Tags:** architecture, security, business-model

---

## Context

OpenHaven needs to decide who owns the underlying cloud infrastructure (AWS accounts, GCP projects, etc.) where user services are deployed.

There are three possible models:

### Option A: OpenHaven Owns Infrastructure (Sub-Account Model)

- OpenHaven creates and owns cloud accounts
- Users are provisioned as sub-accounts or tenants
- OpenHaven handles all billing and passes costs to users
- Users get delegated access to dashboards

### Option B: Users Create Accounts, Give Credentials

- Users create their own cloud accounts
- Users provide OpenHaven with API keys/credentials
- OpenHaven provisions infrastructure in user accounts
- Users own billing and infrastructure

### Option C: Delegated IAM (Role-Based Access)

- Users create their own cloud accounts
- Users grant OpenHaven limited IAM roles via OAuth/delegation
- OpenHaven never stores root credentials
- OpenHaven provisions only required resources
- Users can revoke access anytime

---

## Decision

**We choose Option C: Delegated IAM (Role-Based Access)**

Users own their cloud accounts. OpenHaven uses delegated IAM permissions to provision infrastructure in user-owned accounts.

---

## Rationale

### Why Option C?

#### 1. True User Sovereignty

- Users legally own the infrastructure
- Users control billing directly
- Users can revoke OpenHaven access anytime
- If OpenHaven shuts down, user infrastructure continues running
- Aligns with core principle: "Deploy your personal cloud, own it forever"

#### 2. Trust and Transparency

- OpenHaven never has root access to user accounts
- Limited IAM roles are auditable
- Users can see exactly what permissions are granted
- No hidden access or backdoors
- Builds trust with privacy-conscious users

#### 3. Liability and Compliance

- Users are responsible for their own infrastructure
- OpenHaven is not a data processor under GDPR
- Reduces legal liability for OpenHaven
- Users maintain compliance with their own requirements
- Clear separation of responsibilities

#### 4. Cost Transparency

- Users see actual cloud costs directly
- No markup or hidden fees
- Users can optimize costs themselves
- No surprise bills from OpenHaven
- Users benefit from cloud provider credits/discounts

#### 5. Removability

- Users can stop using OpenHaven without data loss
- Infrastructure remains intact
- Users can take over manual management
- Terraform configs are exportable
- No vendor lock-in to OpenHaven

### Why NOT Option A?

**Contradicts core philosophy:**
- OpenHaven would legally control infrastructure
- Users don't truly "own" their cloud
- If OpenHaven shuts down, users lose everything
- Creates vendor lock-in
- Violates "sovereignty first" principle

**Legal and liability issues:**
- OpenHaven becomes a data processor
- GDPR compliance burden
- Liable for user data breaches
- Responsible for abuse/illegal content
- Requires extensive legal infrastructure

**Business complexity:**
- Must handle billing and payments
- Must manage cloud provider relationships
- Must handle chargebacks and disputes
- Requires financial infrastructure
- Higher operational overhead

### Why NOT Option B?

**Security concerns:**
- Storing user credentials is risky
- Single point of failure if OpenHaven is breached
- Users must trust OpenHaven with root access
- No easy way to rotate credentials
- Scary for non-technical users

**User experience:**
- Managing and sharing credentials is complex
- Users may not understand security implications
- Credential rotation is manual and error-prone
- No standard OAuth flow

---

## Consequences

### Positive

✅ **User Sovereignty** - Users truly own their infrastructure  
✅ **Trust** - Limited, auditable permissions build confidence  
✅ **Removability** - OpenHaven can be removed without data loss  
✅ **Cost Transparency** - Users see actual cloud costs  
✅ **Legal Simplicity** - Clear separation of responsibilities  
✅ **Security** - No root credentials stored  
✅ **Compliance** - Users maintain their own compliance  

### Negative

❌ **Onboarding Friction** - Users must create cloud accounts  
❌ **Complexity** - IAM role setup requires technical knowledge  
❌ **Support Burden** - Must help users with cloud account setup  
❌ **Multi-Provider Complexity** - Different IAM models per provider  
❌ **Limited Control** - Can't optimize infrastructure we don't own  

### Neutral

⚖️ **Business Model** - Must monetize via convenience, not infrastructure markup  
⚖️ **User Responsibility** - Users responsible for their own costs and compliance  
⚖️ **Documentation** - Must provide excellent guides for cloud account setup  

---

## Implementation

### Phase 1: AWS IAM Roles (v0.2.0)

1. User creates AWS account
2. User runs CloudFormation template to create IAM role
3. IAM role grants OpenHaven limited permissions:
   - EC2: Create/manage VMs
   - S3: Create/manage buckets
   - Route53: Manage DNS records
   - IAM: Create service-specific roles
4. User provides role ARN to OpenHaven
5. OpenHaven assumes role to provision infrastructure

### Phase 2: GCP Service Accounts (v0.3.0)

1. User creates GCP project
2. User creates service account
3. User grants limited permissions to service account
4. User provides service account key to OpenHaven
5. OpenHaven uses service account to provision infrastructure

### Phase 3: Multi-Provider Abstraction (v0.4.0)

- Abstract IAM operations behind provider interface
- Support AWS, GCP, Azure, Hetzner, DigitalOcean
- Unified onboarding flow across providers
- Automated IAM role creation where possible

---

## Alternatives Considered

### Hybrid Model

Allow both user-owned and OpenHaven-managed infrastructure.

**Rejected because:**
- Adds complexity to codebase
- Confuses users about ownership model
- Dilutes core message of sovereignty
- Requires maintaining two separate systems

### Terraform Cloud Model

Use Terraform Cloud's remote execution model.

**Rejected because:**
- Adds dependency on Terraform Cloud (SaaS)
- Violates "no SaaS dependencies" principle
- Users still need to create cloud accounts
- Doesn't simplify the core problem

---

## Related Decisions

- [ADR-002: No SaaS Dependencies](./ADR-002-no-saas-dependencies.md)
- Future: ADR on multi-cloud provider support
- Future: ADR on credential management

---

## References

- [AWS IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [GCP Service Accounts](https://cloud.google.com/iam/docs/service-accounts)
- [Terraform Cloud Remote Execution](https://www.terraform.io/cloud-docs/run/remote-operations)
- [GDPR Data Processor Requirements](https://gdpr-info.eu/art-28-gdpr/)

---

## Review Schedule

This decision should be reviewed:
- Before v1.0.0 release
- If user feedback indicates significant onboarding friction
- If legal/compliance requirements change
- Annually as part of architecture review
