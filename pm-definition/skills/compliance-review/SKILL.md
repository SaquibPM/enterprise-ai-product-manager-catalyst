---
description: "Master security and compliance review for enterprise features. SOC 2, GDPR, HIPAA, SOX, FedRAMP frameworks. Data classification, privacy impact assessment, multi-tenant isolation. Stop treating compliance as a checkbox. Build it into product from day 1."
pushy: "Compliance delays are silent killers. A feature fails security review 2 weeks before launch = 2-week slip. A data leak after launch = millions in fines, customer churn, reputation damage. Build compliance muscle now."
purpose: "Systematically review features for compliance risk before engineering starts building."
---

# Security and Compliance Review for Enterprise Features

## Purpose
Compliance is not a department. It's a product discipline. Every feature touches data, connects systems, changes access controls, or alters audit trails. Any of these can create compliance risk.

Enterprise customers operate under strict regulatory regimes:
- Financial services: SOX (Sarbanes-Oxley), requires immutable audit logs
- Healthcare: HIPAA, requires encryption and access controls
- EU customers: GDPR, requires data residency and right to erasure
- US government: FedRAMP, requires certified infrastructure
- Finance teams: SOC 2 Type II, requires independent audit of controls

If you ship a feature without thinking about compliance, you create:
- Security review delays (feature is done; security team takes 4 weeks to review)
- Customer churn (customer's compliance team says "we can't use this"; deal is lost)
- Financial penalties (GDPR violation = 4% of global revenue fine)
- Incident response (security breach ties up all hands for weeks)

This skill teaches you to review features for compliance BEFORE engineering starts, avoid surprise blockers, and build trust with enterprise customers.

## Key Concepts

### 1. The Compliance Review Process

Every new feature that touches data should go through this process:

```
BEFORE ENGINEERING STARTS:
├─ Step 1: Identify data it touches
├─ Step 2: Classify data (public/internal/confidential/restricted)
├─ Step 3: Identify applicable regulations (SOC 2, GDPR, HIPAA, SOX, FedRAMP)
├─ Step 4: Complete Privacy Impact Assessment (PIA)
├─ Step 5: Review multi-tenant isolation
├─ Step 6: Get Security team sign-off

DURING ENGINEERING:
├─ Security team reviews code (optional; only for high-risk features)
├─ Document all compliance decisions

BEFORE LAUNCH:
├─ Final compliance checklist review
├─ Security team verifies implementation matches PIA
├─ Document any deviations
```

**Timeline**: Compliance review should take 1-2 weeks. If you wait until feature is built, you add 4 weeks of rework.

### 2. Data Classification Framework

Before assessing compliance risk, classify the data.

**Public Data**:
- No confidentiality requirement
- Can be shared with anyone
- Examples: Product documentation, marketing materials, public company information
- Compliance impact: Minimal

**Internal Data**:
- Confidential within company
- Not for external sharing
- Examples: Internal process documentation, non-sensitive employee info, internal metrics
- Compliance impact: Low

**Confidential Data**:
- Sensitive business information
- Competitors seeing this would be harmful
- Examples: Customer contract terms, supplier pricing, internal revenue numbers
- Compliance impact: Medium
- Handling: Encrypt at rest, control access by role, audit trail required

**Restricted Data**:
- Highly sensitive; regulatory/legal requirement to protect
- Personal data (GDPR), healthcare data (HIPAA), financial data (SOX), government data (FedRAMP)
- Examples: Customer credit card numbers, employee SSNs, patient medical records, government agency approvals
- Compliance impact: High
- Handling: Encryption in transit (TLS 1.2+) and at rest (AES-256), multi-factor auth, immutable audit logs, right to erasure, data residency controls

**Classification Decision Tree**:
```
START: "What data does this feature touch?"

Q1: Is it publicly available information?
├─ Yes: PUBLIC
└─ No: Continue

Q2: Is it confidential business information?
├─ Yes: CONFIDENTIAL
└─ No: Continue

Q3: Is it personal data (employee names, customer emails, etc.)?
├─ Yes: Likely RESTRICTED (if in EU) or CONFIDENTIAL (if not subject to GDPR)
└─ No: Continue

Q4: Is it financial data, healthcare data, or government data?
├─ Yes: RESTRICTED
└─ No: Continue

Q5: Is it non-sensitive internal info?
├─ Yes: INTERNAL
└─ No: CONFIDENTIAL (when in doubt, over-classify)
```

### 3. Regulatory Framework Mapping

Different regulations apply to different companies and data types. Use this matrix to identify which regulations apply.

**SOC 2 Type II** (All SaaS companies)
- Applies to: All companies offering cloud services; customers need proof of controls
- Data types: All customer data (company is responsible for protecting it)
- Key controls:
  - Access control: Document who can access what; enforce based on roles
  - Encryption: Data in transit (TLS 1.2+) and at rest (AES-256)
  - Audit logging: Document all access, changes, deletions; immutable storage
  - Change management: Document feature rollout; test in staging before production
  - Incident response: Have a plan; communicate with customers
- Compliance check: Annual SOC 2 audit; auditor reviews controls

**GDPR** (All EU customers)
- Applies to: Companies processing personal data of EU residents
- Personal data: Name, email, phone, address, IP address, cookie IDs, etc.
- Key requirements:
  - Consent: Get explicit consent before processing personal data
  - Data residency: EU personal data must be stored in EU data centers
  - Right to erasure: Customer can request deletion; you have 30 days to delete
  - Right to access: Customer can request copy of their data; you have 30 days to provide
  - Data Processing Agreement (DPA): Customer must sign this before you process their data
  - Privacy Impact Assessment (PIA): For any processing of personal data
  - Breach notification: If personal data is breached, notify customers within 72 hours
- Compliance check: Regular audits; DPA enforcement

**HIPAA** (Healthcare companies)
- Applies to: Companies processing Protected Health Information (PHI)
- PHI: Patient names, SSNs, medical records, diagnoses, treatment plans, etc.
- Key requirements:
  - Business Associate Agreement (BAA): Healthcare customer must sign this before you handle PHI
  - Encryption: All PHI encrypted in transit and at rest
  - Access control: Limit access to PHI to authorized personnel only
  - Audit logging: Log all access to PHI
  - Data segregation: PHI must be segregated from non-PHI data
  - Breach notification: Notify customer within 24 hours if PHI is breached
- Compliance check: Annual HIPAA audit; BAA enforcement

**SOX** (Public companies & financial services)
- Applies to: Public companies, financial institutions, companies handling financial data
- Financial data: Invoices, purchase orders, contracts, pricing, revenue, etc.
- Key requirements:
  - Immutable audit trail: All financial transactions must be logged; logs cannot be modified
  - Access controls: Segregation of duties (person who creates PO cannot approve it)
  - Change management: All changes to financial systems documented, tested, approved
  - Internal controls: Regular review of access logs; identify and remediate violations
- Compliance check: Annual SOX audit by external auditor

**FedRAMP** (Government contractors)
- Applies to: Companies providing cloud services to US government
- Data: Government agency data, contractor proposals, contract details
- Key requirements:
  - FISMA compliance: Federal Information Security Management Act
  - Security controls: NIST-based security controls (extremely rigorous)
  - Continuous monitoring: Real-time security monitoring
  - Incident response: 24-hour incident notification to government
  - Data residency: Government data stored in US data centers only
- Compliance check: Annual FedRAMP audit by authorized assessment organization (3PAO)

### 4. Privacy Impact Assessment (PIA) Template

For any feature that touches personal data or restricted data, complete a PIA before development starts.

**PIA Template**:

```
PRIVACY IMPACT ASSESSMENT

Feature: [Feature Name]
Date: [Date]
Completed by: [PM Name]
Reviewed by: [Security/Privacy Officer]

1. DATA COLLECTION
   What personal data does this feature collect?
   - Customer names? (Yes/No)
   - Email addresses? (Yes/No)
   - Phone numbers? (Yes/No)
   - IP addresses? (Yes/No)
   - Credit card data? (Yes/No)
   - Employee data? (Yes/No)
   - Other? ____

2. DATA PROCESSING
   What does the feature do with the data?
   - Display it to users? (Yes/No)
   - Store it in database? (Yes/No)
   - Send it to third-party service? (Yes/No) [If yes, name service and get DPA]
   - Use for analytics/ML? (Yes/No) [If yes, anonymize before using]
   - Delete it after X days? (Yes/No) [If yes, when?]

3. DATA RETENTION
   How long is personal data retained?
   - While customer is active: ____ days
   - After customer deletion: ____ days (must be deleted by then)
   - For audit logs: ____ days
   - For compliance holds: ____ days

4. REGULATORY APPLICABILITY
   Which regulations apply to this feature?
   - GDPR (EU customers)? (Yes/No)
   - HIPAA (healthcare data)? (Yes/No)
   - SOX (financial data)? (Yes/No)
   - FedRAMP (government)? (Yes/No)
   - Other? ____

5. COMPLIANCE CONTROLS REQUIRED
   What controls must be implemented?
   [ ] Encryption in transit (TLS 1.2+)
   [ ] Encryption at rest (AES-256)
   [ ] Access control (role-based)
   [ ] Audit logging (immutable)
   [ ] Data residency (EU/US)
   [ ] Right to erasure (30-day deletion)
   [ ] Right to access (export data on demand)
   [ ] Multi-factor auth (for admin access)
   [ ] Data Processing Agreement (DPA)
   [ ] Business Associate Agreement (BAA)

6. RISKS & MITIGATIONS
   What are potential compliance risks?
   Risk 1: Personal data could be exposed if database is breached
   Mitigation: Encrypt all personal data at rest; limit database access; regular security testing

   Risk 2: Customer might request data deletion; we might not comply in 30 days
   Mitigation: Implement automated deletion process; test monthly; document audit trail

7. THIRD-PARTY INTEGRATIONS
   Does this feature integrate with external services?
   Service 1: [Email service]
   - What data is sent? [Email address, name, feature usage data]
   - Is service GDPR-compliant? [Yes/No - verify with DPA]
   - Is data encrypted in transit? [TLS 1.2+]
   - Where is data stored? [US/EU/Other]

8. SIGN-OFF
   Security team: ____________  Date: ____
   PM: ____________  Date: ____
   Engineering lead: ____________  Date: ____
```

### 5. Multi-Tenant Isolation Checklist

Multi-tenant is the default for enterprise SaaS. If isolation is broken, one customer's data leaks to another = GDPR violation = major churn.

**Isolation Checklist**:

```
DATABASE LEVEL:
[ ] Every table has tenant_id column
[ ] Every query filters by tenant_id
[ ] Row-level security enforced (database constraints or ORM)
[ ] No queries that accidentally return cross-tenant data
[ ] Tested: Tenant A queries return only Tenant A's data, not Tenant B's

API LEVEL:
[ ] All API endpoints check auth token; extract tenant_id from token
[ ] Every query to database filtered by tenant_id from auth token
[ ] User cannot specify ?tenant=OTHER_TENANT and access other tenant's data
[ ] API rejects cross-tenant requests with 403 Forbidden
[ ] Tested: Tenant A user cannot access Tenant B's data via API

AUTH LEVEL:
[ ] API keys are tenant-specific; one key cannot access multiple tenants
[ ] User session tokens contain tenant_id; verified on every request
[ ] Session tokens expire; cannot be replayed indefinitely
[ ] Tested: Tenant A API key does not grant access to Tenant B's data

AUDIT LOGGING:
[ ] Every action logs which tenant performed the action
[ ] Audit logs are filtered by tenant_id
[ ] Tenant A cannot see Tenant B's audit logs
[ ] Tested: Tenant A's audit export contains only their own actions

BACKUP/RESTORE:
[ ] Database backups are encrypted
[ ] Backup restore process enforces tenant_id verification
[ ] Cannot accidentally restore Tenant A's backup to Tenant B's environment
[ ] Tested: Backup/restore maintains tenant isolation

CACHE:
[ ] Redis/Memcached entries include tenant_id in key
[ ] Cache keys enforce tenant_id namespace
[ ] One tenant's cache invalidation doesn't affect other tenants
[ ] Tested: Tenant A cache misses don't return Tenant B's data

FILE STORAGE:
[ ] S3/storage paths include tenant_id
[ ] Tenant A cannot read files in Tenant B's directory
[ ] Access control on storage enforces tenant_id
[ ] Tested: Tenant A cannot list/read Tenant B's files

WEBHOOKS/EXPORTS:
[ ] Data exports (CSV, JSON) filtered by tenant_id
[ ] Webhook events only sent to the correct tenant's webhook endpoint
[ ] Tested: Tenant A export contains only Tenant A's data
```

### 6. Audit Trail Requirements

SOX and SOC 2 require immutable audit trails. Design this in from the start.

**Audit Trail Specification**:

```
AUDIT TRAIL SCHEMA:

For every action in the system, log:
- event_id: Unique identifier (UUID)
- timestamp: When action occurred (ISO 8601, UTC)
- tenant_id: Which tenant performed the action
- user_id: Who performed the action (user or system)
- action_type: What action was performed (create, update, delete, approve, reject, etc.)
- resource_type: What was affected (contract, invoice, supplier, etc.)
- resource_id: Which resource was affected
- change_details: What changed (old value → new value)
- ip_address: Where action came from
- user_agent: What app/browser they used
- status: Success or failure?

IMMUTABILITY GUARANTEE:
[ ] Once written to audit log, cannot be modified
[ ] Cannot be deleted (except with explicit deletion audit event that itself is audited)
[ ] Implementation: Database trigger that prevents UPDATE/DELETE on audit table, or use append-only log

RETENTION POLICY:
[ ] Live retention: 90 days in hot storage (fast access)
[ ] Archive retention: 2 years in cold storage (slower access, lower cost)
[ ] Legal hold: If customer is in litigation, extend retention indefinitely
[ ] Deletion: After retention expires, crypto-shred the data (securely erase encryption keys)

AUDIT LOG EXPORT:
[ ] Customers can export audit logs for their tenant
[ ] Export is timestamped; proves it was taken at specific time
[ ] Export is cryptographically signed; prevents tampering
[ ] SOX auditor can verify: Audit logs are complete, unmodified, and retained per policy

TESTING:
[ ] Automated test: Insert audit log entry; verify it cannot be modified
[ ] Automated test: Export audit log; verify it matches database
[ ] Manual test: Attempt to delete audit log entry; verify it fails
```

---

## Application: Step-by-Step Compliance Review

### Scenario: AI-Powered Document Analysis Feature

**Use Case**: Customer uploads contracts (PDFs). AI reads contract text, extracts key terms (amount, dates, signatories), identifies risks (liability caps, indemnification).

### Step 1: Identify Data It Touches

```
Data touched by this feature:
- Customer contract PDFs (proprietary business documents)
- Extracted contract metadata (amounts, dates, parties, terms)
- AI processing metadata (which contracts were analyzed, when, accuracy scores)
- Supplier names (from contracts)
- Financial terms (contract amounts)
- User audit trail (who uploaded, when, what was extracted)
```

### Step 2: Classify Data

```
Data | Classification | Reason | Handling |
------|-----------------|--------|-----------|
Contract PDFs | Confidential | Proprietary business docs | Encrypt at rest, control access, audit log access |
Extracted metadata | Confidential | Contains contract terms | Encrypt at rest, control access, audit log changes |
AI metadata | Internal | Processing info; no PII | Standard database encryption |
Supplier names | Confidential | Competitive info | Encrypt, control access |
Financial terms | Confidential | Proprietary pricing | Encrypt, control access |
User audit trail | Restricted (if GDPR applies) | Contains user actions | Immutable log, retention policy, right to erasure |
```

### Step 3: Identify Applicable Regulations

```
Applicable regulations:

SOC 2 Type II:
- All customer data must be protected
- Access must be logged
- Audit trail must be immutable

GDPR (if any EU customers or if contract PDFs contain EU personal data):
- If PDFs contain employee names (EU residents), is GDPR personal data
- Must comply with data residency (store in EU data center)
- Must support right to erasure (delete within 30 days)
- Must have DPA with customer

HIPAA (if healthcare customer uploads healthcare contracts):
- Healthcare contracts might contain patient references
- If so, PHI handling required (encryption, BAA, audit trail)

SOX (if financial services customer):
- Contract amounts are financial data
- Audit trail must be immutable
- Access controls must separate duties
```

### Step 4: Complete Privacy Impact Assessment

```
PRIVACY IMPACT ASSESSMENT: AI Document Analysis Feature

Feature: AI-Powered Contract Analysis
Date: 2026-01-15
Completed by: [PM Name]
Reviewed by: [Security Officer]

1. DATA COLLECTION
   What personal data does this feature collect?
   - Customer names? (Yes - from contract signature lines)
   - Email addresses? (Yes - from contract header/contact info)
   - Employee data? (Possibly - from contract approval chains)

2. DATA PROCESSING
   What does the feature do with the data?
   - Display it to users? (Yes - shows extracted terms including names)
   - Store it in database? (Yes - extracted metadata stored)
   - Send it to third-party service? (Yes - AI service receives contract text to analyze)
   - Use for analytics/ML? (Possibly - improve model on anonymized data)
   - Delete it after X days? (No - retained per customer retention policy)

3. DATA RETENTION
   How long is personal data retained?
   - While customer is active: Per customer's contract (typically 7 years)
   - After customer deletion: 30 days (must be deleted by then per GDPR)
   - For audit logs: 90 days hot, 2 years cold
   - For compliance holds: Indefinite if customer is in litigation

4. REGULATORY APPLICABILITY
   Which regulations apply to this feature?
   - GDPR (EU customers)? (Yes - contract PDFs may contain EU personal data)
   - HIPAA (healthcare data)? (Possible - if customer handles healthcare contracts)
   - SOX (financial data)? (Yes - contract amounts are financial data)
   - FedRAMP (government)? (No - not for government market yet)

5. COMPLIANCE CONTROLS REQUIRED
   What controls must be implemented?
   [X] Encryption in transit (TLS 1.2+)
   [X] Encryption at rest (AES-256)
   [X] Access control (role-based; only contract analysts can see PDFs)
   [X] Audit logging (log who accessed what contract, when)
   [X] Data residency (EU contracts stored in EU data center)
   [X] Right to erasure (delete PDFs and extracted data within 30 days on request)
   [X] Right to access (export extracted data on demand)
   [X] Multi-factor auth (for admin access to contract data)
   [X] Data Processing Agreement (DPA with AI service provider)

6. RISKS & MITIGATIONS
   Risk 1: Extracted contract metadata could be exposed if database breached
   Mitigation: Encrypt all extracted metadata at rest (AES-256); limit database access to 3 admins; quarterly penetration testing

   Risk 2: AI service provider could retain contract text to improve model (privacy violation)
   Mitigation: Include clause in DPA: "Provider cannot retain customer data for model training; must delete within 24 hours"

   Risk 3: Customer requests right to erasure; we don't delete within 30 days
   Mitigation: Implement automated deletion workflow; test monthly; document in audit log

   Risk 4: Contract PDFs contain employee personal data; we transfer to EU customer (GDPR violation)
   Mitigation: PIA identifies: Customers own the data in contracts; customer is responsible for obtaining consent; we just store/process

   Risk 5: One customer's contract accidentally visible to another tenant (data leakage)
   Mitigation: Implement and test multi-tenant isolation; every query filters by tenant_id; quarterly audit

7. THIRD-PARTY INTEGRATIONS
   Does this feature integrate with external services?
   
   Service 1: AI Model Provider (OpenAI, Anthropic, etc.)
   - What data is sent? Contract text (PDFs converted to text; no metadata)
   - Is service GDPR-compliant? (Yes - OpenAI has DPA)
   - Is data encrypted in transit? (Yes - TLS 1.2+)
   - Where is data stored? (OpenAI stores in US; must verify with DPA if EU customers acceptable)
   - Retention: Contract terms say "API requests not retained for training"; verify in DPA

   Service 2: Blob Storage (S3)
   - What data is stored? Contract PDFs
   - Is service GDPR-compliant? (Yes - AWS has DPA; supports EU data residency)
   - Is data encrypted? (Yes - AES-256 server-side encryption)
   - Where is stored? (EU customers → eu-west-1 region; US customers → us-east-1)

8. SIGN-OFF
   Security team: [Security Officer signature]  Date: 2026-01-15
   PM: [PM signature]  Date: 2026-01-15
   Engineering lead: [Eng Lead signature]  Date: 2026-01-15
   
   STATUS: APPROVED - Ready for development with controls implemented above
```

### Step 5: Multi-Tenant Isolation Review

```
MULTI-TENANT ISOLATION REVIEW: AI Document Analysis

Database Level:
[X] contracts table has tenant_id column
[X] extracted_terms table has tenant_id column
[X] audit_logs table has tenant_id column
[X] Every query filters by tenant_id
[X] Row-level security enforced: database constraint prevents cross-tenant reads
[X] Test passed: Tenant A queries return only Tenant A contracts, not Tenant B

API Level:
[X] All endpoints check auth token; extract tenant_id from token signature
[X] POST /api/v1/contracts filters results by tenant_id from token
[X] User cannot submit ?tenant=OTHER_TENANT in request; ignored, filtered by token
[X] Attempt to access other tenant's contract returns 403 Forbidden
[X] Test passed: Tenant B API key cannot read Tenant A contracts

Audit Logging:
[X] Every upload logs {tenant_id, user_id, contract_id, timestamp}
[X] Audit logs filtered by tenant_id
[X] Tenant A cannot query audit logs for Tenant B
[X] Test passed: Tenant A audit export contains only their own uploads

File Storage (S3):
[X] S3 path includes tenant_id: s3://contracts-prod/tenant-123/contract-abc.pdf
[X] Bucket policy enforces prefix-level access by tenant_id
[X] Tenant A credentials cannot list/read files in tenant-456/ directory
[X] Test passed: Tenant A cannot see Tenant B's contracts in S3

STATUS: ISOLATION VERIFIED - Ready for deployment
```

### Step 6: Audit Trail Specification

```
AUDIT TRAIL SPECIFICATION: AI Document Analysis

For every contract action, log:
{
  "event_id": "evt_contract_upload_001",
  "timestamp": "2026-01-15T10:30:00Z",
  "tenant_id": "tenant_123",
  "user_id": "user_john_456",
  "action_type": "contract_uploaded",
  "resource_type": "contract",
  "resource_id": "contract_abc123",
  "change_details": {
    "filename": "Supplier Agreement.pdf",
    "file_size_bytes": 102400,
    "upload_duration_ms": 2500,
    "ai_processing_triggered": true
  },
  "ip_address": "192.168.1.100",
  "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)",
  "status": "success"
}

Immutability:
[ ] Database trigger prevents UPDATE on audit_events table
[ ] Database trigger prevents DELETE on audit_events table
[ ] Only deletion allowed: INSERT deletion event (immutable record that data was deleted)
[ ] Test: Attempt UPDATE on audit row → fails with database constraint error

Retention Policy:
[ ] Hot storage (90 days): PostgreSQL archive_timeout = 90 days
[ ] Cold storage (2 years): Archive to S3 Glacier
[ ] Legal hold: Query shows which contracts are under legal hold; extend retention indefinitely
[ ] Deletion: After 2 years, crypto-shred (delete S3 object + encryption keys)

Export for SOX Audit:
[ ] SOX auditor requests: "Show all contract uploads for 2025"
[ ] System exports: audit_events WHERE action_type='contract_uploaded' AND year(timestamp)=2025
[ ] Export is timestamped and signed with company's private key
[ ] Auditor verifies: Signature matches (proves export wasn't tampered with)

Testing:
[X] Test 1: Insert audit log; verify UPDATE fails
[X] Test 2: Insert audit log; verify DELETE fails
[X] Test 3: Manually run deletion_event; verify original event still exists (only deletion event added)
[X] Test 4: Export audit logs; verify completeness and signature
```

---

## Worked Example: Compliance Review for Multi-Tenant AI Feature

### Scenario: Supplier Risk Scoring

A feature that automatically scores supplier risk based on:
- Historical contract data (contract amounts, terms, defaults)
- Public data (company news, financial reports)
- Proprietary model (trained on aggregated customer data)

### Compliance Questions

**Question 1: Is this feature transferring data to external parties?**

Yes. The feature uses an external AI model provider (OpenAI or similar) to analyze contract text.

**Compliance concern**: Contract text is confidential business data. If we send it to OpenAI unencrypted or without data processing agreement, we violate GDPR and customer trust.

**Mitigation**:
- Get data processing agreement (DPA) with AI provider
- DPA must state: "Provider will not retain customer data for training"
- Encrypt contract text in transit (TLS 1.2+)
- In request to AI provider, strip identifying information (remove customer name, only send anonymized contract terms)

**Question 2: Are we training models on customer data?**

Possibly. If we use customer contracts to improve our risk scoring model, we're training on customer data.

**Compliance concern**: GDPR prohibits processing personal data (customer names in contracts) without consent. SOX requires that financial data (contract terms) remain confidential.

**Mitigation**:
- Option A (Recommended): Don't train on customer data. Use only historical aggregated data.
- Option B: Get explicit customer consent before using their data for model training.
- Option C: Anonymize contract data before using for training (remove all identifiers; use only financial terms and outcomes).

**Question 3: Could supplier risk scores leak customer data?**

Yes, indirectly. If a supplier sees their risk score is high, they infer negative information about the customer (e.g., we think they're a bad payer, we think they breach contracts).

**Compliance concern**: Supplier learns confidential information about customer's payment behavior = data leakage.

**Mitigation**:
- Risk scores are internal-only; never share with suppliers
- If feature is shared with suppliers later, ask: Does supplier need to know their score? Can we share without revealing why (just "recommended" or "approved")?

**Question 4: What data is classified as "restricted"?**

Contract amounts and terms are financial data (SOX requires protection). Supplier performance history is competitive intelligence (confidential). Customer approval chains may contain employee names (GDPR personal data if EU).

**Classification**:
```
Data | Classification | Why | Handling |
------|-----------------|-----|-----------|
Contract amounts | Restricted | SOX financial data | Encrypt at rest, immutable audit trail |
Contract terms | Confidential | Proprietary business data | Encrypt at rest, access control |
Supplier identifiers | Confidential | Competitive info | Encrypt at rest, access control |
Risk scores | Internal | Derived metric | Standard encryption |
Approval chains | Restricted (if EU) | Employee names = GDPR personal data | Encrypt, right to erasure |
```

**Question 5: Does multi-tenant isolation work for risk scores?**

Risk scores are derived from aggregated historical data. If we train a single global model on all customers' data, is that a privacy violation?

**Compliance concern**: Tenant A's contracts influence risk scores for Tenant B (data leakage).

**Mitigation**:
- Option A (Recommended): Train separate models per tenant (higher cost; strong isolation)
- Option B: Train one global model but ensure no tenant-specific data leaks
  - Model uses only anonymized features (contract amount, term length, industry) not tenant identifiers
  - Risk scores don't reveal training data
  - Test: Can we reverse-engineer which customer's contracts influenced a risk score? If yes, it's a privacy violation. If no, it's safe.
- Option C: Aggregate training data across customers but randomize temporal order (prevent inference of which customer is which)

**Decision**: Option B (global model with anonymized features) is acceptable if anonymization is validated.

---

## Compliance Review Checklist

Use this checklist for every feature.

```
COMPLIANCE REVIEW CHECKLIST

Feature: _______________________
Date: _______________________
Completed by: _______________________

1. DATA IDENTIFICATION
[ ] Identified all data this feature touches
[ ] Classified each data type (public/internal/confidential/restricted)
[ ] Identified if feature involves personal data (GDPR)
[ ] Identified if feature involves financial data (SOX)
[ ] Identified if feature involves healthcare data (HIPAA)
[ ] Identified if feature involves government data (FedRAMP)

2. REGULATORY ASSESSMENT
[ ] Determined which regulations apply
[ ] Reviewed SOC 2 Type II requirements (if applicable)
[ ] Reviewed GDPR requirements (if applicable)
[ ] Reviewed HIPAA requirements (if applicable)
[ ] Reviewed SOX requirements (if applicable)
[ ] Reviewed FedRAMP requirements (if applicable)

3. PRIVACY IMPACT ASSESSMENT
[ ] Completed PIA template (Section 4 above)
[ ] Identified data retention requirements
[ ] Identified right to erasure/access requirements
[ ] Identified DPA requirements (if using third parties)
[ ] Identified BAA requirements (if healthcare)

4. MULTI-TENANT ISOLATION
[ ] Reviewed database-level isolation (tenant_id filtering)
[ ] Reviewed API-level isolation (auth token enforcement)
[ ] Reviewed audit logging isolation (logs filtered by tenant)
[ ] Reviewed file storage isolation (S3 path prefix)
[ ] Tested: Can Tenant A access Tenant B data? (Should be: No)

5. AUDIT TRAIL
[ ] Defined what actions must be logged
[ ] Defined immutability requirements (cannot be modified)
[ ] Defined retention policy (how long to keep)
[ ] Defined deletion process (how to comply with right to erasure)
[ ] Tested: Can audit logs be modified? (Should be: No)

6. ENCRYPTION
[ ] Verified TLS 1.2+ for data in transit
[ ] Verified AES-256 for data at rest
[ ] Verified encryption key management (keys not hard-coded, rotated regularly)
[ ] Verified: Can data be read if database is stolen? (Should be: No, encrypted)

7. ACCESS CONTROL
[ ] Defined who can create/read/update/delete this data
[ ] Defined role-based access (admin/analyst/viewer)
[ ] Implemented role checks in code (not just UI)
[ ] Tested: Can viewer user modify data? (Should be: No)

8. THIRD-PARTY SERVICES
[ ] Listed all third-party services this feature uses
[ ] Verified DPA (Data Processing Agreement) with each service
[ ] Verified: What data is sent to third party? (Should be: Only necessary data)
[ ] Verified: How long does third party retain data? (Should be: Minimal)
[ ] Tested: Can third party see customer identifying information? (Should be: No, anonymized)

9. INCIDENT RESPONSE
[ ] Defined: If this feature has a security issue, who do we notify? (Customer, internal security team, etc.)
[ ] Defined: How quickly must we notify? (24 hours, 72 hours per GDPR, etc.)
[ ] Defined: What do we tell customers? (Transparent communication required)

10. SIGN-OFF
[ ] Security team reviewed and approved
[ ] PM confirmed all controls are feasible within timeline
[ ] Engineering lead confirmed all controls will be implemented
[ ] Feature is ready for development

STATUS: ☐ APPROVED  ☐ APPROVED WITH CONDITIONS  ☐ REJECTED

If conditions or rejected: Document why and required actions before re-review.
```

---

## Common Pitfalls

### Pitfall 1: Compliance as an Afterthought
**Anti-Pattern**: "Feature is 80% built. Now let's do a security review."

**Why it fails**: Compliance issues discovered mid-development = weeks of rework. You're 4 weeks away from launch; now it's 10 weeks. Customer misses renewal window. Deal lost.

**Right way**: Compliance review BEFORE engineering starts.
- Week 1: PM writes PRD + compliance review
- Week 2: Security approves; engineering understands requirements
- Week 3: Engineering builds with compliance built in
- Week 8: Feature ships; no surprise delays

### Pitfall 2: Treating Compliance as a Checkbox
**Anti-Pattern**: "Do we have encryption?" → "Yes" → "Compliance approved!"

**Why it fails**: Encryption is table stakes. The real question is: Is encryption implemented correctly? Are keys rotated? Can admins access encrypted data without authorization?

**Right way**: Dig deeper.
```
Not this: [ ] Encryption implemented
But this: [ ] Data encrypted at rest (AES-256, FIPS-validated)
          [ ] Encryption keys stored separately (AWS KMS, not hard-coded)
          [ ] Keys rotated every 90 days
          [ ] Admin access to encrypted data logged and audited
          [ ] Tested: Can database admin read customer data without auth token? (No)
```

### Pitfall 3: Ignoring Multi-Tenant Risks
**Anti-Pattern**: "Our database has tenant_id column; multi-tenant is solved."

**Why it fails**: One forgotten WHERE clause = cross-tenant data leakage.

Example:
```
Bad:
SELECT * FROM contracts WHERE status = 'approved'  /* Forgot: AND tenant_id = current_tenant */

Result: Customer A sees all customers' approved contracts (privacy violation)
```

**Right way**: Test every query.
```
For every query touching customer data:
[ ] Query explicitly filters by tenant_id
[ ] Test: Does query return data from other tenants if tenant_id is wrong? (Should be: No)
[ ] Code review: Did reviewer verify WHERE clause includes tenant_id?
[ ] Automated test: Cross-tenant query attempt returns 403 Forbidden
```

### Pitfall 4: Data Classification Confusion
**Anti-Pattern**: "Everything is confidential; everything needs encryption."

**Why it fails**: Over-encrypting everything is expensive and slow. Not all data needs the same protection level.

**Right way**: Use the classification framework (Section 2).
```
Public data (no encryption needed):
- Product documentation, marketing copy, public pricing

Internal data (standard encryption):
- Internal metrics, non-sensitive docs, internal process info

Confidential data (encryption + access control):
- Customer terms, supplier pricing, internal metrics that would hurt if public

Restricted data (encryption + audit trail + right to erasure):
- Personal data, financial data, healthcare data, government data
```

### Pitfall 5: GDPR Surprise at Enterprise Deals
**Anti-Pattern**: "We signed a customer in EU. Now they ask: Where is our data stored? Do you comply with GDPR?"

**Why it fails**: You say "We store in US; no GDPR support." Customer can't use your product (GDPR violation). Deal closes at $0.

**Right way**: Design GDPR support from day 1.
```
For any feature touching personal data:
[ ] If EU customers use it, data is stored in EU data center (Ireland or Frankfurt)
[ ] Right to erasure is implemented (delete within 30 days on request)
[ ] Right to access is implemented (export data on demand)
[ ] Data Processing Agreement is in customer contract
```

### Pitfall 6: No Audit Trail Design
**Anti-Pattern**: "We log user actions in standard database table."

Customer asks: "Can you prove who approved this contract? When?" 

You check logs. Someone deleted the log entry. You can't prove anything. Customer fails audit. Churn.

**Why it fails**: Without immutability, logs are worthless for compliance.

**Right way**: Design immutable audit trails.
```
[ ] Audit logs stored in append-only table (no UPDATE/DELETE allowed)
[ ] Deletion is itself an audit event (documented deletion; original log still visible)
[ ] Audit logs encrypted and backed up (cannot be lost)
[ ] Export includes hash/signature (proves logs weren't tampered with after export)
[ ] Tested: Attempt to UPDATE audit log → fails with database error
```

### Pitfall 7: Third-Party Risk Ignored
**Anti-Pattern**: "We use OpenAI for feature X. They're a big company; must be secure."

Reality: You don't have a Data Processing Agreement with OpenAI. They could theoretically train models on your customer data (they say they don't, but it's not contractually prohibited).

**Why it fails**: GDPR violation. Customer finds out. Churn. Fines.

**Right way**: Get DPA.
```
For every third-party service:
[ ] Verify: Does service have GDPR DPA? (AWS, Azure: yes. Unknown vendors: no.)
[ ] Verify: DPA includes "data retention" clause (service must delete within X days)
[ ] Verify: DPA includes "sub-processors" clause (who else has access?)
[ ] Document in contract: If we use service X, customer must approve
```

### Pitfall 8: Right to Erasure = Deletion Hell
**Anti-Pattern**: "Customer requests right to erasure. We delete their data from database. But they're still in backups, audit logs, analytics."

Customer later sues: "You didn't fully delete my data (GDPR violation)."

**Why it fails**: GDPR right to erasure is stricter than just soft-deletes. You must delete from:
- Primary database
- Backups (or wait for backup rotation)
- Audit logs (can't delete audit log; must mark as deleted and not include in exports)
- Analytics (anonymize or delete)
- Third parties (must ask AI service, email service, etc. to delete)

**Right way**: Design deletion process.
```
[ ] Deletion marked in primary database (logical delete; not physical)
[ ] Backups: Either backup rotation (wait 30 days for old backup to expire) or crypto-shred (delete encryption key)
[ ] Audit logs: Cannot delete audit records; mark as "under erasure request"; exclude from exports
[ ] Analytics: Anonymize historical records; cannot identify customer
[ ] Third parties: Send deletion request; verify deletion
[ ] Tested: 30 days after deletion, verify customer data is unrecoverable from all systems
```

---

## References

- **SOC 2 Trust Service Criteria**: AICPA guide to SOC 2 controls
- **GDPR Articles 32-35**: Data protection, DPA, DPIA requirements
- **HIPAA Security Rule**: Access controls, encryption, audit logging
- **SOX Section 302**: Internal controls over financial reporting
- **FedRAMP Security Controls**: NIST-based security requirements
- **OWASP Data Protection Cheat Sheet**: Industry best practices
- **Anthropic Constitution AI**: Framework for AI safety (relevant for AI features)

---

## Next Steps

1. **Add compliance review to your process**: Insert before development starts
2. **Create DPA templates**: Standard Data Processing Agreements for customers and vendors
3. **Implement audit logging**: Make sure all features log actions immutably
4. **Test multi-tenant isolation**: For every feature, verify tenants can't see each other's data
5. **Document regulatory requirements**: Create templates for SOC 2, GDPR, HIPAA, SOX, FedRAMP compliance
6. **Hire/partner with compliance**: If you don't have a Chief Information Security Officer (CISO) or privacy officer, get one
7. **Run annual audits**: SOC 2, GDPR, HIPAA, SOX audits verify your controls work
8. **Build security culture**: Make compliance everyone's job, not just one team's
9. **Monitor for changes**: Regulations evolve; quarterly review of regulatory landscape
10. **Learn from incidents**: If security breach occurs, use it to improve controls
