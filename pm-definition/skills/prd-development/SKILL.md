---
description: "Master PRD writing for enterprise B2B SaaS with compliance frameworks, buyer-vs-user personas, multi-tenant architecture considerations, and migration strategies. Stop writing solution specs and start writing outcome-driven requirements."
pushy: "Your PRD is a contract with engineering. Enterprise deals fail because PRDs don't address compliance, buyer personas are missing, and rollout strategy is assumed. Build the muscle here."
purpose: "Write PRDs that survive enterprise customer scrutiny, security reviews, and product-market-fit validation."
---

# PRD Development for Enterprise B2B SaaS

## Purpose
A PRD is not a solution spec. It is a contract between product, engineering, security, and sales. In enterprise B2B, a weak PRD costs money: delayed closures, engineering rework, missed compliance deadlines, and customer churn at renewal. This skill teaches you to write PRDs that account for the multi-tenant complexity, regulatory stakes, and buyer-vs-user split of enterprise software.

## Key Concepts

### 1. The Enterprise PRD Mandate
Enterprise PRDs must solve for:
- **Compliance Impact**: GDPR, SOC 2, HIPAA, SOX, FedRAMP. What audit trails, data handling, or access controls does this feature require?
- **Buyer vs User Personas**: The CFO (buyer) and the AP clerk (user) have different goals. Your PRD must address both or the deal fails at legal/procurement review.
- **Multi-Tenant Data Isolation**: How does this feature handle tenant separation? Can one customer's data leak to another's?
- **Integration Dependencies**: What APIs or integrations must be stable for this feature to work? What breaks if an external API changes?
- **Migration/Rollout Strategy**: How do you turn this on for existing customers without breaking their workflows?
- **Contract Commitments**: Is this feature promised in any existing enterprise contract? When is it due?

### 2. PRD Section Architecture
A complete enterprise PRD has these sections:

#### Header
```
Title: [Feature Name]
Product Line: [Core/AI/Infrastructure]
Target Personas: [Buyer], [User], [Admin]
Release Target: [Quarter/Date]
Status: [Draft/Approved/In Dev/In Review]
Approval Chain: [PM], [Eng Lead], [Security], [Sales Lead]
```

#### Business Context (The Why)
- **Problem Statement**: What market gap or customer pain point does this solve?
- **Business Metrics**: Revenue impact? Customer retention? TAM expansion?
- **Competitive Context**: What are competitors doing? (Don't copy; use to define differentiation.)
- **Strategic Alignment**: How does this tie to 12-month product strategy?

#### Buyer Personas (The Who Pays)
Document the procurement gatekeepers:
- **Primary Buyer**: Title, buying process, cost center, approval authority
- **Buyer Pain Points**: Budget constraints, audit/compliance concerns, contract lifecycle
- **Buyer Success Metrics**: How does the buyer measure success? (Cost per transaction, audit time saved, risk reduction)

#### User Personas (The Who Uses)
Document the day-to-day operators:
- **Primary User**: Title, workflows, pain points
- **User Success Metrics**: How does the user measure success? (Time saved, accuracy, fewer errors)
- **Non-Goals for Users**: What should we NOT do? (Over-complexity, feature bloat)

#### Compliance & Security Section
For every feature, ask:
- **Data Classification**: What data does this feature touch? (Public/Internal/Confidential/Restricted)
- **Regulatory Impact**: GDPR (right to erasure, data residency), SOC 2 (audit trails), HIPAA (PHI encryption), SOX (access controls), FedRAMP (government compliance)
- **Multi-Tenant Isolation**: How is tenant data separated? What prevents data leakage?
- **Access Control**: Who can create/read/update/delete? Role-based or attribute-based?
- **Audit Trail Requirements**: What must be logged? For how long?
- **Third-Party Risk**: Does this integrate with external APIs or SaaS tools? What are the security implications?

#### Acceptance Criteria
For each criterion, define testable behavior:
- **Functional**: The feature does X, Y, Z under conditions A, B, C
- **Performance**: P95 latency <500ms, throughput >10k requests/sec
- **Compliance**: Audit log captures all user actions with timestamps; SOC 2 Type II attestation is maintained
- **Enterprise**: Feature works for tenants with >10k users; no performance degradation
- **Backwards Compatibility**: Existing integrations continue to work; no breaking API changes

#### Multi-Tenant & Scale Architecture
- **Tenant Isolation Model**: How are tenants separated? (Database row, schema, separate DB, separate infrastructure)
- **Scalability Targets**: What's the max throughput per tenant? Per system?
- **Migration Path**: How does this feature roll out to existing customers?
- **Sunset Plan**: If we're replacing a legacy system, how do we shut it down?

#### Integration & API Contract
- **Outbound APIs**: If this feature calls external systems, specify rate limits, retry logic, fallback behavior
- **Inbound APIs**: If this feature is consumed by other systems, specify versioning strategy, breaking change policy
- **Webhook Events**: If this feature emits events, define event schema, delivery guarantees, retry policy

#### Rollout & Migration Strategy
- **Phased Rollout**: Roll out to 1% of tenants first, then 25%, then 100%? Why?
- **Opt-In vs Opt-Out**: Does the feature ship enabled or disabled by default?
- **Customer Communication**: What do we tell customers? When?
- **Rollback Plan**: If something breaks, how do we roll back without data loss?
- **Data Migration**: If this touches existing data, how do we migrate without breaking existing workflows?

#### Success Metrics
Define metrics for each stakeholder:
- **Business**: Revenue impact, upsell velocity, churn rate
- **User**: Adoption rate, feature usage frequency, NPS improvement
- **Engineering**: Bug escape rate, support ticket volume, system stability

#### Non-Goals
Explicitly state what this PRD does NOT include. This prevents scope creep and sets expectations.

#### Future Phases
Roadmap items that are out of scope for v1 but planned for v2/v3.

---

## Application: Step-by-Step PRD Development

### Step 1: Start with Buyer Personas
Before writing acceptance criteria, understand who signs the check.

**Example**: For AI-powered contract risk scoring in a procurement platform:
- **Primary Buyer**: Procurement Director at Fortune 500 company
  - Budget: $50-100k annually
  - Pain: Currently uses 3 tools (contract review, risk scoring, audit logging); wants consolidation
  - Approval: Needs CFO sign-off, Security/Legal review
  - Success: Reduces contract review time by 40%, cuts legal review cycles
  
- **Secondary Buyer**: General Counsel
  - Pain: No audit trail of who reviewed/approved contracts
  - Success: Full audit trail for SOX compliance

### Step 2: Map Compliance Requirements
For each regulatory framework your customer uses, define feature requirements:

**GDPR**: 
- If this feature processes personal data (employee names, emails), you need explicit consent and right to erasure
- If contract data includes personal data, document data handling
- Data residency: Where is data stored? (EU data must stay in EU)

**SOC 2 Type II**:
- Audit logging: Log who accessed what contracts, when, with what changes
- Access control: Implement role-based access (Admin, Reviewer, Approver, Viewer)
- Encryption: Data in transit (TLS 1.2+) and at rest (AES-256)
- Change management: Document feature rollout as a change event

**Multi-Tenant Isolation**:
- Company A's contracts must never be visible to Company B
- One tenant's risk scores must not influence another tenant's scoring model
- Query filters must enforce tenant boundaries on every API call

### Step 3: Write Acceptance Criteria in Testable Format
Bad: "The system should handle large volumes of contracts"
Good:
- Given a tenant with 50,000 contracts, when a user filters contracts by risk score, the API returns results in <1 second (P95 latency)
- Given a tenant with 10 concurrent users uploading contracts, the system processes 100 contracts per minute without data loss

### Step 4: Define Integration Points
For each external system, specify the contract:

**Outbound: Email notification service**
- Trigger: When a contract risk score exceeds 75, notify contract owner
- API: POST /send-email with {recipient, subject, body, contractId}
- Retry: 3 retries with exponential backoff; fail silently if email service is down after 3 retries (don't block contract workflow)
- Rate Limit: <100 requests/second across all tenants; <10 requests/second per tenant

**Inbound: Contract data from ERP (e.g., SAP)**
- Webhook: ERP sends contract data changes to POST /api/v1/contracts/webhook
- Schema: {contractId, supplierId, amount, terms, lastModified}
- Versioning: Support v1 (current) and v2 (new schema) for 6 months; then deprecate v1
- Breaking Change Policy: Email customers 60 days before sunset

### Step 5: Plan the Rollout
For a new feature, don't flip the switch for all 10k customers on day 1.

**Phased Rollout**:
- Week 1: Enable for 1% of customers (100 customers); monitor error logs and performance
- Week 2: Enable for 10% if Week 1 is stable
- Week 3: Enable for 50%
- Week 4: Enable for 100%

**Opt-In Strategy**:
- Feature ships disabled by default
- Admin sees a UI toggle: "Enable AI Risk Scoring (Beta)"
- Customers who opt in get onboarding video and support from CSM
- Collect feedback and telemetry; use to refine for GA

---

## Worked Example: AI-Powered Contract Risk Scoring for Enterprise Procurement

### Header
```
Title: AI-Powered Contract Risk Scoring
Product Line: Core AI
Target Personas: Procurement Director (Buyer), Legal Counsel (Buyer), Contract Analyst (User)
Release Target: Q2 2026 (GA), Q1 2026 (Pilot)
Status: Approved
Approval Chain: [Saquib Jawed - PM], [Eng Lead - Elena], [Security - Marcus], [Sales - Priya]
```

### Business Context

**Problem Statement**:
Procurement teams at Fortune 500 companies spend 4-6 weeks reviewing supplier contracts manually. They use fragmented tools: document review (Microsoft Word), risk databases (internal spreadsheets), and legal review (email). This process is slow (contract-to-signature takes 60+ days), error-prone (critical terms missed), and expensive (external legal review costs $3k-10k per contract).

**Market Opportunity**:
- TAM: 5,000 enterprises globally with >$1B revenue; 80% are our target
- Customer research: 12 procurement directors reported 50% time savings would justify $100k annual spend
- Competitive gap: Competitors (Icertis, Apptio) offer risk scoring but not in-platform; users must switch tools

**Business Metrics**:
- Revenue: Tier existing customers to add this feature at +$20k/year (increases ARPU by 8%)
- Retention: Contract review pain is #3 churn driver; solving this increases NRR by 3-5%
- Expansion: 60% of upsell conversation starts with contract workflow pain; this creates 12-month expansion pipeline of $2M+

**Strategic Alignment**:
Supports 2026 strategy: "Make Zycus the platform of record for source-to-pay, not just procurement."

### Buyer Personas

**Primary Buyer: Procurement Director**
- Title: VP of Procurement or Chief Procurement Officer
- Company: Fortune 500, $10B+ revenue, 200+ active suppliers
- Buying Process: 
  - Identifies problem (contract review bottleneck)
  - Requests demo from Sales
  - Initiates 3-month pilot with 100 contracts
  - If successful, approves budget for full deployment (typically $50-100k annual)
  - Requires CFO approval if >$50k; requires Legal/Security sign-off
- Pain Points:
  - Current process: Route contracts through email → Legal team reviews → Risk flagging via spreadsheet → Procurement adds to metadata
  - Too many tools = no single source of truth; contracts get lost
  - Suppliers complain about 60+ day turnaround; Procurement loses deals to faster competitors
  - Compliance: No audit trail of who approved what; fails SOX spot checks
- Success Metrics:
  - Reduce contract-to-signature time from 60 days to 30 days (50% improvement)
  - Reduce legal review cycles from 3 to 1 (2/3 reduction)
  - Eliminate "lost contract" incidents (currently 8-10 per year)
  - Full audit trail for SOX compliance

**Secondary Buyer: General Counsel**
- Title: General Counsel or Chief Legal Officer
- Gatekeeping Power: Approves feature before deployment (security/compliance concerns)
- Pain Points:
  - Regulatory exposure: No audit trail of contract approvals; can't prove due diligence in audit
  - Risk management: High-risk contracts slip through because no one flags them systematically
  - Integration hell: No visibility into contract lifecycle once it leaves Legal for execution
- Success Metrics:
  - 100% audit trail: Who accessed contract, when, what changes, who approved
  - Automatically flag high-risk terms (indemnification, liability caps, termination clauses)
  - SOX-compliant access controls (Approve, Review, View roles)

**User Persona: Contract Analyst**
- Title: Senior Procurement Analyst or Contract Manager
- Workflows:
  - Day 1: Receive contract from supplier in email
  - Day 2-5: Manually extract key data (amount, terms, milestones) into spreadsheet
  - Day 5: Submit to Legal for review
  - Day 10-20: Incorporate Legal feedback; revise contract
  - Day 20-30: Get supplier sign-off; route to Finance for payment setup
- Pain Points:
  - Manually extracting data is error-prone (typos in contract amount = costly mistakes)
  - No visibility: Can't tell where a contract is in the review cycle (Legal? Finance? Stuck in email?)
  - Manual risk flagging: No systematic way to identify risky supplier terms until Legal points them out (reactive, not proactive)
- Success Metrics:
  - AI auto-extracts key data; analyst verifies instead of entering
  - Real-time dashboard: See which contracts are in Legal, waiting for Legal feedback, ready for execution
  - Risk flags appear on-screen immediately; analyst can address before sending to Legal

### Compliance & Security Section

**Data Classification**:
- Contract documents: Confidential (proprietary business terms, supplier rates)
- Extracted risk scores: Confidential (competitive intelligence if leaked)
- Audit logs: Restricted (must be tamper-proof for SOX compliance)

**Regulatory Impact**:

**GDPR**:
- Scope: Contracts may contain employee names (approvers, signers), email addresses
- Requirements:
  - Data Privacy Impact Assessment (DPIA) required before launch
  - Contracts stored in EU data center if customer is EU-based
  - Right to erasure: If an employee leaves, their name/email must be redacted from contract records
  - Retention: Contracts retained for 7 years per customer policy, then deleted

**SOC 2 Type II**:
- Audit logging: Every action logged
  - Event: {user_id, action (create/read/update/delete), contract_id, timestamp, ip_address, change_details}
  - Retention: 90 days live, 2 years archived
  - Immutable: Logs cannot be modified (database transaction integrity)
- Access control: Role-based
  - Admin: Create users, manage tenants, view all contracts
  - Approver: Create/edit contracts, approve for execution
  - Reviewer: Read contracts, add comments, request changes
  - Viewer: Read-only access to contracts
- Encryption: TLS 1.2+ in transit, AES-256 at rest
- Change management: Feature rollout documented as change event; testing in staging before production

**HIPAA** (if customer processes healthcare contracts):
- Scope: Healthcare contracts may contain PHI (patient treatment plans, medical terms)
- Requirements:
  - Business Associate Agreement (BAA) required before customer can use feature
  - Contracts with PHI must be encrypted; only authorized users can view
  - Access log: Who accessed contracts with PHI, when, why (for audit)

**Multi-Tenant Isolation**:
- Database design: Each tenant's contracts stored in rows with tenant_id
- Query enforcement: All queries filtered by tenant_id; no exceptions
  - Bad: SELECT * FROM contracts WHERE risk_score > 75
  - Good: SELECT * FROM contracts WHERE risk_score > 75 AND tenant_id = :current_tenant_id
- API enforcement: Auth middleware checks token for tenant_id; rejects requests for other tenants
- Secrets isolation: Each tenant's API keys separated; one tenant's key doesn't grant access to another's data
- Audit scope: Audit logs filtered by tenant_id; tenant A can't see tenant B's access logs
- AI model: Risk scoring model is shared across tenants (cost efficiency) but doesn't leak training data between tenants
  - Implementation: Model trained on anonymized, aggregated contract data; no tenant-specific signals leak

**Third-Party Risk: Email/Notification Service**:
- Risk: If email service is compromised, contract data could be exfiltrated in notification payloads
- Mitigation:
  - Never include full contract text in email; include only contract ID and link
  - Link is time-limited (valid for 24 hours) and requires authentication
  - Email service is SOC 2 Type II certified; contract addendum required

**Access Control Matrix**:
```
| Action        | Viewer | Reviewer | Approver | Admin |
|---------------|--------|----------|----------|-------|
| View contract | Own    | All      | All      | All   |
| Edit contract |        | Own      | All      | All   |
| Approve       |        |          | Yes      | Yes   |
| Delete        |        |          |          | Yes   |
| View audit    | Own    | Own+team | All      | All   |
```

### Acceptance Criteria

**Functional AC**:
1. Given a contract PDF uploaded to the system, when an AI model processes it, it extracts {supplier_name, contract_amount, term_length, renewal_date, key_risk_terms} with >95% accuracy
2. Given extracted data, when the system scores risk, it outputs {risk_score (1-100), risk_category (low/medium/high), flagged_terms (list of high-risk clauses)}
3. Given a contract marked "high risk", when a Reviewer opens it, risk flags are prominently displayed with explanations
4. Given an Approver reviews a contract, when they click "Approve", the action is logged to audit trail with {user_id, timestamp, contract_id, decision}
5. Given a contract in "Approved" status, when the user clicks "Send for Signature", a workflow email is triggered to supplier with 24-hour link (no full contract text in email body)

**Performance AC**:
1. Given 100 contracts queued for processing, when the system processes them, P95 processing time is <2 minutes per contract (CPU time for AI model, not including network latency)
2. Given a procurement director viewing dashboard with 50k contracts, when they filter by "high risk", results load in <1 second (P95)
3. Given 500 concurrent users accessing contracts, system maintains <200ms P95 API latency for read requests

**Compliance AC**:
1. Given a contract accessed by a Reviewer, when the Reviewer views it, the system logs {user_id, timestamp, action_type, contract_id, ip_address, user_agent} to immutable audit table
2. Given a contract with GDPR-sensitive data (employee names), when the contract is marked for deletion, the system redacts employee names before permanent deletion
3. Given a SOC 2 audit, when auditor requests access logs for a tenant, logs are complete and unmodified for past 90 days

**Enterprise AC**:
1. Given a customer with 500k contracts in the system, when they query by supplier name, results load in <1 second (P95)
2. Given a tenant with 1000 concurrent users, when 100 of them upload contracts simultaneously, system maintains 99.9% availability; no user sees errors
3. Given feature rollout to 100 tenants, when we monitor for 48 hours, system experiences zero unplanned downtime; all tenants can process contracts normally

**Backwards Compatibility AC**:
1. Given existing API consumers calling GET /api/v1/contracts, when they request the endpoint, response includes new field `risk_score` with null value (backward compatible)
2. Given a customer with custom integrations (e.g., contract data sent to external system), when the integration continues to use old API, it works without modification

### Multi-Tenant & Scale Architecture

**Tenant Isolation Model**: 
Row-level security with tenant_id foreign key on every table. No schema separation (cost-efficient for 1000s of tenants).

**Scalability Targets**:
- Per-tenant throughput: 1000 contracts/day (peak)
- Per-system throughput: 1M contracts/day (aggregated across all tenants)
- Concurrent users per tenant: 500 (simultaneous dashboard viewers + API users)
- Contract repository size: 100M contracts total across all tenants

**Migration Path for Existing Customers**:
- Phase 1 (Week 1-2): Feature ships disabled; no customer sees it
- Phase 2 (Week 3): Opt-in pilot with 20 pilot customers; beta label; no SLA
- Phase 3 (Week 5): GA launch; feature enabled by default for new customers; existing customers see "Enable AI Risk Scoring" toggle
- Phase 4 (Week 8): 95%+ of customers have enabled; marketing campaign highlights value
- Sunset: Legacy manual risk flagging (spreadsheet) continues to work in parallel for 6 months; then removed in v2.0

**Data Migration for Existing Contracts**:
- Existing contracts (already in system) do NOT get auto-scored in bulk; would overwhelm AI infrastructure
- Instead: Lazy scoring. When user views a contract, if risk_score is null, trigger AI scoring in background (async, <5 min)
- User sees "Risk score calculating..." placeholder; dashboard shows spinner
- This prevents the "all contracts scored instantly" spike that would break the system

### Integration & API Contract

**Outbound: AI Risk Scoring Service**
- Internal service that consumes contract text and returns risk score
- Contract: POST /internal-api/risk-score with {contract_text, contract_id, tenant_id}
- Response: {risk_score (1-100), risk_category, flagged_terms ([])}
- Rate limit: 1000 requests/second across system; 50 requests/second per tenant
- Retry: 3 retries with exponential backoff; if all retries fail, cache is cleared and next request tries fresh
- Fallback: If service is down for >30 min, feature degraded to "No risk score available"; doesn't block user

**Outbound: Notification Service (Email)**
- Contract: POST /notify with {user_id, event_type (contract_awaiting_approval), metadata (contract_id, contract_amount)}
- Email template: "Contract from [Supplier] awaiting your approval. [Link] [Expires in 24 hours]"
- Rate limit: 100 emails/second; 10 emails/second per tenant
- Retry: 2 retries; if both fail, log incident but don't block contract workflow
- Privacy: Never include full contract text in email body; only ID and link

**Inbound API: Contract Data**
- GET /api/v1/contracts: List contracts (paginated, filtered by risk_score, status, supplier)
  - Response includes new field: `risk_score` (integer 1-100 or null if not yet scored)
  - Backward compatible: Old API clients see null; new clients see score
- GET /api/v1/contracts/{id}: Get single contract with risk details
  - Response: {id, supplier_id, amount, term_length, risk_score, risk_category, flagged_terms, created_at, updated_at}
- Versioning: v1 (current, support until Q4 2026); v2 (future, different risk scoring model)
- Breaking Change Policy: 90-day deprecation notice before removing v1

**Webhook Events**:
- Event: `contract.risk_scored` (emitted after AI scores a contract)
- Payload: {contract_id, risk_score, risk_category, flagged_terms, timestamp}
- Delivery: HTTP POST to customer-configured webhook URL; retry 3x if 5xx; no retry if 4xx
- Usage: Customers can integrate with Risk Management systems to auto-alert security teams if risk_score > 80

### Rollout & Migration Strategy

**Phased Rollout**:
```
Week 1 (5% of customers = ~50 customers):
- Enable for 5% of tenant base
- Monitor: Error logs, API latency, AI model accuracy (does it correctly identify high-risk terms?)
- Success criteria: <0.1% error rate; P95 latency <2 min; accuracy >90%
- Owner: On-call engineer + PM

Week 2 (25% of customers):
- Expand if Week 1 stable
- Alert: Send email to enrolled customers: "AI Risk Scoring now available"
- CSM outreach: Call top 20 customers in cohort; offer 30-min training session

Week 3 (100% of customers):
- Full rollout; feature enabled by default
- Marketing: Blog post "Introducing AI Risk Scoring"; LinkedIn post; in-app banner
- Support readiness: Support team briefed on common questions; FAQ drafted
```

**Opt-In Strategy**:
- Feature ships disabled for existing customers
- New customers: Feature enabled by default (they expect it)
- Existing customers: See "Enable AI Risk Scoring" toggle in Settings
- Incentive: First 100 contracts scored free; then included in subscription
- Feedback loop: Collect NPS on feature; iterate model based on user feedback

**Customer Communication Plan**:
- Email (Week 0): "Coming soon: AI Risk Scoring"
- Blog post (Week 1): Technical deep dive; how it works; privacy/security details
- In-app banner (Week 1): "New: AI Risk Scoring" with link to documentation
- CSM calls (Week 2): 1-on-1 with top 50 customers; offer training
- Support FAQ (Week 1): 10 common questions and answers

**Rollback Plan**:
- If error rate exceeds 5% in any rollout phase, immediately disable for that cohort
- Customers already using feature: Feature continues to work; no data loss
- Data integrity: Risk scores already computed are not deleted; they persist for audit purposes
- Communication: Incident post-mortem sent to affected customers within 24 hours

**Data Migration**:
- Contracts created before feature launch: No bulk scoring
- Lazy scoring: When user views contract, if risk_score is null, trigger async scoring
- Ensures: No system overload; users see placeholder while scoring completes
- Edge case: If contract is deleted before scoring completes, async task gracefully cancels

### Success Metrics

**Business Metrics**:
- Revenue: 60% of pilot customers upsell to add-on (targets $2M pipeline)
- Retention: Churn rate for customers who enable feature is <5% (vs. 12% baseline)
- NRR: +2% improvement for feature-enabled cohort

**User Metrics**:
- Adoption: 80% of pilot customers enable feature within 30 days of access
- Engagement: 50% of enabled customers score 100+ contracts in first 90 days
- Feature usage: Average risk score viewed 2x per contract (user confirms AI recommendation)

**Engineering Metrics**:
- Reliability: 99.9% uptime for feature; <5 unplanned incidents in first 90 days
- Performance: P95 processing latency <2 min; P95 API query latency <1 sec
- Accuracy: AI model >95% accuracy on risk term detection (measured against human reviewers)

**Compliance Metrics**:
- Audit readiness: 100% of actions logged to immutable audit table
- Data isolation: 0 cross-tenant data exposures (measured quarterly)
- Security: 0 unplanned data breaches; all SOC 2 audit findings resolved

### Non-Goals
- This feature does NOT replace human legal review; it supports it
- This feature does NOT create binding risk decisions; risk scores are advisory
- This feature does NOT support contract execution or e-signature (existing feature)
- This feature does NOT support contract spend analysis or savings tracking (separate feature)

### Future Phases
- **v2**: Multi-language contract scoring (support French, German, Spanish contracts)
- **v2**: Custom risk scoring rules (enterprise can define their own risk logic)
- **v3**: Integration with external risk databases (D&B, supplier credit scores)
- **v3**: Supplier risk tiering (High/Medium/Low) across contract portfolio

---

## Common Pitfalls

### Pitfall 1: Solution Spec Instead of Problem Spec
**Anti-Pattern**: "Build a dashboard showing real-time contract risk scores with filters for supplier, status, and risk level. Use a red-green-yellow color scheme. Add a CSV export button."

**Why it fails**: You've described the UI, not the problem. If engineering reads this, they'll build exactly what you drew. Then the CFO asks, "Why can't I see contracts at risk of payment default?" and you have to rebuild.

**Right way**: "Contract managers spend 4-6 weeks identifying at-risk contracts because there's no centralized view of contract terms and compliance status. We need a way to identify, surface, and prioritize high-risk contracts for legal review in under 1 week."

Then engineering can propose: Dashboard, API, email alerts, or something you didn't think of.

### Pitfall 2: Missing Compliance Section
**Anti-Pattern**: PRD has no mention of GDPR, SOC 2, data retention, or audit trails. Engineering ships feature. Then General Counsel asks, "Who accessed this contract? When? What did they change?" and there's no audit log.

**Why it fails**: In enterprise, compliance is non-negotiable. Features without audit trails fail security review and delay closures by 3-6 months. You've lost the deal.

**Right way**: For every feature, ask:
- What data does this touch? (PII, financial, healthcare?)
- What regulations apply? (GDPR, SOC 2, HIPAA, SOX?)
- What's the audit trail requirement? (Who accessed what, when, for how long to retain?)
- How do we prevent cross-tenant data leakage?

Write these into the PRD. Engineering must address them.

### Pitfall 3: Buyer Persona Missing
**Anti-Pattern**: PRD focuses on user workflows (Contract Analyst pain) but ignores buyer (Procurement Director).

**Why it fails**: Procurement Director approves budget and contract. If the feature doesn't address their success metrics (reduce cycle time, audit compliance, cost per contract), they won't pay. You've built something users like but buyers don't fund.

**Right way**: For every enterprise feature, map:
- Who buys it? (Title, cost center, approval authority)
- What's their success metric? (Cost savings, risk reduction, compliance, headcount reduction?)
- What's their pain if this feature fails? (Missed SLA? Audit failure? Customer churn?)

Then ensure your feature delivers on their metric.

### Pitfall 4: Acceptance Criteria That Aren't Testable
**Anti-Pattern**: "The system should handle contract risk scoring efficiently and scale to large numbers of contracts."

**Why it fails**: Engineering ships it. You ask, "Did it meet acceptance criteria?" They say yes. Then in production, P95 latency is 10 seconds and customers complain. No one can prove anyone failed because "efficiently" is subjective.

**Right way**: Define testable criteria with specific numbers:
- P95 latency for risk scoring: <2 minutes per contract
- Concurrent users: System supports 500 users per tenant with <200ms P95 API latency
- Accuracy: AI model identifies high-risk terms with >95% precision

Now when testing is done, you can run a test and verify: Pass or Fail.

### Pitfall 5: No Multi-Tenant Isolation Thinking
**Anti-Pattern**: PRD says "Build risk scoring" but doesn't specify how tenant data is isolated.

Engineering makes a choice: Shared database with tenant_id filter on queries.

Then in production, a bug causes Customer A's contracts to appear in Customer B's dashboard for 30 minutes. GDPR violation. Churn. The deal is lost.

**Right way**: For every data-touching feature, explicitly state:
- Tenant isolation model: Row-level (tenant_id on each row), schema-level (separate schema per tenant), or database-level
- Query enforcement: All queries must filter by tenant_id
- Audit scope: Can Tenant A see Tenant B's audit logs? (Never.)
- Secrets: Are API keys shared or isolated per tenant?

Write these into the PRD. Security review approves before engineering starts.

### Pitfall 6: Rollout Strategy = "Release to Everybody"
**Anti-Pattern**: Feature is ready; flip the switch for all 10k customers on a Tuesday morning.

**Why it fails**: Bug in the risk scoring model causes 5% of contracts to be marked "High Risk" incorrectly. 500 customers are now confused. Support gets 100 tickets. NPS drops 5 points. The customer who just renewed for $500k is ready to churn.

**Right way**: Phased rollout:
- Week 1: Enable for 5% (pilot customers); monitor error logs and model accuracy
- Week 2: Enable for 25% if Week 1 is stable
- Week 3: Enable for 100%

This way, you catch bugs affecting 5% of customers, not 100%.

### Pitfall 7: Integration Without Fallback
**Anti-Pattern**: Risk scoring depends on external AI service. If service is down, feature is down.

**Why it fails**: External service has a 2-hour outage. Customers can't upload contracts. Support gets flooded. SLA breach.

**Right way**: Design fallbacks:
- If AI service is down for <30 min: Show "Risk score calculating..." placeholder; user can proceed without risk score
- If AI service is down for >30 min: Feature degrades to manual risk entry; AI returns when service recovers

This way, feature is always available; degraded but available.

### Pitfall 8: No Success Metrics
**Anti-Pattern**: Feature ships. You assume it's working because engineering says so.

**Why it fails**: After 3 months, you discover users enabled the feature but rarely use it. Feature-enabled customers have the same churn as others. You spent 3 months on something that didn't move the needle.

**Right way**: Define success metrics before launch:
- Adoption: % of customers who enable feature
- Engagement: % of contracts that are scored
- Business impact: Churn rate for feature-enabled customers
- Accuracy: AI model precision on risk detection

At month 1, review. If adoption is 20% instead of target 60%, investigate why. Maybe the feature is hard to use. Maybe users don't trust the AI. Iterate.

---

## References

- **Marty Cagan, "Inspired"**: How to write problem statements, not solution specs
- **Reforge PM101**: Acceptance criteria frameworks
- **SOC 2 Trust Service Criteria**: Audit logging, access control requirements
- **GDPR Articles 32-35**: Data processing, DPIA requirements
- **Inspired by Zycus customer research**: Interviews with 15 Procurement Directors and General Counsels

---

## Next Steps

1. **Use the template above for your next PRD**: Copy the Header, Business Context, Buyer Personas sections into your next feature doc
2. **Compliance checklist**: Before submitting to Engineering, complete the Compliance & Security section; get Security team sign-off
3. **Buyer persona interview**: Talk to 3 customers who would pay for this feature; validate buyer success metrics match reality
4. **Acceptance criteria workshop**: With Engineering Lead, go through each AC and agree on how to test it
5. **Rollout plan with Engineering**: Design phased rollout together; identify what metrics signal "go" vs "stop"
