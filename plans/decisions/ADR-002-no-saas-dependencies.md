# ADR-002: Zero SaaS Dependencies

**Status:** Accepted  
**Date:** 2026-02-22  
**Deciders:** Core Team  
**Tags:** architecture, philosophy, sovereignty

---

## Context

OpenHaven must decide whether to use proprietary SaaS services for convenience (email relay, authentication, AI APIs, monitoring, etc.) or rely solely on open source software and raw cloud infrastructure.

### Common SaaS Dependencies in Similar Projects

- **Email:** SendGrid, Mailgun, AWS SES, Postmark
- **Authentication:** Auth0, Okta, Firebase Auth
- **AI:** OpenAI API, Anthropic API, Google AI
- **Monitoring:** Datadog, New Relic, Sentry
- **Analytics:** Google Analytics, Mixpanel
- **CDN:** Cloudflare (beyond DNS), Fastly
- **Search:** Algolia, Elasticsearch Cloud
- **Payments:** Stripe, PayPal (for hosted version)

---

## Decision

**OpenHaven will use ZERO proprietary SaaS dependencies.**

All functionality will be provided by:
1. Raw cloud infrastructure (compute, storage, networking)
2. Open source software
3. Self-hosted services
4. Self-hosted models (for AI)

---

## Rationale

### Why Zero SaaS?

#### 1. Sovereignty and Control

**Problem:** SaaS dependencies create external control points.

- Users don't control SaaS provider policies
- SaaS providers can change pricing, terms, or shut down
- Data flows through third-party systems
- Privacy implications of external services

**Solution:** Self-hosted everything means users control the entire stack.

#### 2. Cost Predictability

**Problem:** SaaS costs scale unpredictably.

- Email relay: $X per 1000 emails
- AI API: $X per token
- Monitoring: $X per host
- Costs can spike unexpectedly

**Solution:** Cloud infrastructure costs are predictable and transparent.

#### 3. Vendor Lock-In

**Problem:** SaaS APIs create dependencies.

- Switching providers requires code changes
- Proprietary APIs and data formats
- Migration is painful and risky

**Solution:** Open source software uses standard protocols and formats.

#### 4. Privacy and Trust

**Problem:** SaaS providers see user data.

- Email content flows through relay services
- AI prompts sent to external APIs
- Monitoring data sent to third parties
- Privacy-conscious users won't accept this

**Solution:** All data stays in user-controlled infrastructure.

#### 5. Philosophical Consistency

**Problem:** Using SaaS contradicts OpenHaven's core message.

- "Own your cloud" but email goes through SendGrid?
- "Zero lock-in" but dependent on Auth0?
- Hypocrisy undermines trust

**Solution:** Practice what we preach - full sovereignty.

---

## Consequences

### Positive

✅ **True Sovereignty** - No external dependencies  
✅ **Cost Transparency** - Only pay cloud infrastructure costs  
✅ **Privacy** - All data in user-controlled systems  
✅ **Philosophical Consistency** - Walk the talk  
✅ **No Vendor Lock-In** - Standard protocols only  
✅ **Predictable Costs** - No surprise SaaS bills  
✅ **Trust** - Privacy-conscious users can verify  

### Negative

❌ **Email Deliverability** - Self-hosted email is hard  
❌ **Complexity** - Must implement everything ourselves  
❌ **Maintenance Burden** - More services to manage  
❌ **Initial Setup** - More configuration required  
❌ **Support Burden** - Must help users with complex services  
❌ **Feature Velocity** - Slower than using SaaS APIs  

### Neutral

⚖️ **Tradeoffs** - Sovereignty vs. convenience  
⚖️ **User Responsibility** - Users manage their own services  
⚖️ **Documentation** - Must provide excellent guides  

---

## Specific Decisions

### Email: Self-Hosted with Optional Relay

**Default:** Mailcow (self-hosted mail server)

**Challenges:**
- SPF/DKIM/DMARC configuration
- IP reputation management
- Spam filtering
- Deliverability to Gmail/Outlook

**Our Approach:**
- Automate SPF/DKIM/DMARC setup
- Provide deliverability testing tools
- Document IP warmup procedures
- Allow optional SMTP relay (user-configured, not required)
- Set realistic expectations

**Why not SES/SendGrid by default?**
- Violates sovereignty principle
- Creates SaaS dependency
- Users can configure relay themselves if needed

### Authentication: Self-Hosted SSO

**Choice:** Keycloak (open source identity provider)

**Why not Auth0/Okta?**
- SaaS dependency
- User data flows through third party
- Vendor lock-in
- Costs scale with users

**Keycloak provides:**
- OAuth/OIDC
- SSO across services
- User management
- Self-hosted, full control

### AI: Self-Hosted Models

**Choice:** Ollama + Open WebUI

**Why not OpenAI/Anthropic API?**
- SaaS dependency
- User prompts sent to third party
- Privacy concerns
- Costs scale with usage
- Vendor lock-in

**Self-hosted approach:**
- Run models locally (CPU or GPU)
- User data never leaves infrastructure
- Predictable costs (compute only)
- Can use any model

### Monitoring: Self-Hosted

**Choice:** Prometheus + Grafana (or similar)

**Why not Datadog/New Relic?**
- SaaS dependency
- Infrastructure metrics sent to third party
- Expensive at scale

**Self-hosted approach:**
- Prometheus for metrics
- Grafana for visualization
- Uptime Kuma for status pages
- All data stays local

### DNS: Cloudflare (Exception)

**Choice:** Cloudflare DNS API

**Why Cloudflare?**
- Free tier is generous
- Excellent API
- Not storing user data (just DNS records)
- Can be swapped for other providers
- DNS is inherently distributed

**Is this a SaaS dependency?**
- Technically yes, but minimal
- DNS records are public information
- Easy to migrate to other providers
- User can use any DNS provider
- We just default to Cloudflare for convenience

---

## Implementation Strategy

### Phase 1: Core Services (v0.2-v0.4)

Deploy self-hosted versions of:
- Keycloak (authentication)
- Vaultwarden (passwords)
- Gitea (git hosting)
- Nextcloud (files/calendar)
- Mailcow (email)

### Phase 2: Advanced Services (v0.5+)

Add self-hosted:
- Ollama + Open WebUI (AI)
- Prometheus + Grafana (monitoring)
- Uptime Kuma (status pages)

### Phase 3: Optional Integrations (v1.0+)

Allow user-configured (not required) integrations:
- SMTP relay (if user wants better deliverability)
- Backup services (if user wants off-site backups)
- CDN (if user wants global distribution)

**Key:** These are user-configured, not OpenHaven dependencies.

---

## Exceptions and Nuances

### Cloud Infrastructure is Not SaaS

Using AWS S3, GCP Cloud Run, etc. is NOT a SaaS dependency because:
- These are raw infrastructure services
- No proprietary APIs (S3 is a standard)
- User owns the account and data
- Can be swapped for other providers
- No vendor lock-in

### DNS Providers

DNS is inherently distributed and public. Using Cloudflare DNS is acceptable because:
- DNS records are public information
- Easy to migrate to other providers
- User can choose any DNS provider
- We just default to Cloudflare for convenience

### Future Hosted Control Plane

If we offer a hosted version of OpenHaven's control plane (SaaS-optional model like n8n):
- The control plane is SaaS
- But the services it deploys are NOT
- Users can self-host the control plane
- No lock-in to hosted version

---

## Alternatives Considered

### Hybrid Approach

Use SaaS for hard problems (email relay), self-host everything else.

**Rejected because:**
- Inconsistent philosophy
- Still creates dependencies
- Undermines trust
- Slippery slope to more SaaS

### SaaS-Optional

Make SaaS integrations optional but supported.

**Rejected because:**
- Adds complexity to codebase
- Temptation to rely on SaaS
- Dilutes core message
- Users can configure integrations themselves

### Pragmatic SaaS

Use SaaS where it makes sense, prioritize user experience.

**Rejected because:**
- Contradicts core philosophy
- OpenHaven's value IS sovereignty
- Plenty of pragmatic solutions exist already
- We're building something different

---

## Risks and Mitigations

### Risk: Email Deliverability

**Mitigation:**
- Excellent documentation
- Automated SPF/DKIM/DMARC setup
- Deliverability testing tools
- IP warmup guides
- Realistic expectations
- Allow user-configured relay

### Risk: Complexity Overwhelms Users

**Mitigation:**
- Opinionated defaults
- Automated configuration
- Progressive disclosure
- Excellent documentation
- Community support

### Risk: Feature Velocity Suffers

**Mitigation:**
- Focus on core use cases
- Don't try to compete with SaaS on features
- Compete on sovereignty and control
- Build what matters most

### Risk: Support Burden

**Mitigation:**
- Comprehensive documentation
- Community forums
- Automated diagnostics
- Clear troubleshooting guides

---

## Success Criteria

This decision is successful if:

1. **No SaaS dependencies** in core OpenHaven platform
2. **Email deliverability** is acceptable (>95% to major providers)
3. **User satisfaction** with sovereignty vs. convenience tradeoff
4. **Community trust** in OpenHaven's philosophy
5. **Cost savings** vs. SaaS alternatives (50-70% cheaper)

---

## Related Decisions

- [ADR-001: User-Owned Infrastructure](./ADR-001-user-owned-infrastructure.md)
- Future: ADR on email deliverability strategy
- Future: ADR on monitoring and observability

---

## References

- [Email Deliverability Best Practices](https://www.mail-tester.com/)
- [Keycloak Documentation](https://www.keycloak.org/documentation)
- [Ollama Documentation](https://ollama.ai/docs)
- [12-Factor App Principles](https://12factor.net/)

---

## Review Schedule

This decision should be reviewed:
- Before v1.0.0 release
- If email deliverability proves insurmountable
- If user feedback indicates SaaS is necessary
- Annually as part of architecture review
