---
name: release-notes
title: "Release Notes & Changelogs for Enterprise"
category: "Product Communication"
description: "Master audience-specific release notes, breaking change communication, and API changelogs for enterprise SaaS"
difficulty: "Intermediate"
skills_required: ["Product Knowledge", "Technical Communication", "Customer Empathy"]
duration_minutes: 30
---

# Release Notes & Changelogs for Enterprise

## Purpose

Enterprise customers have different release note expectations than SMB or consumer users. A breaking API change that breaks a single integration can cost you a multi-million dollar customer. Compliance changes need explicit communication. New data models can impact customer reports and dashboards. This skill teaches you how to write audience-specific release notes, handle breaking changes with migration guides, and maintain API changelogs that keep developers happy and legal teams satisfied.

## Key Concepts

### Audience-Specific Release Note Templates

**Template 1: Customer-Facing Release Notes (What customers care about)**

Purpose: Communicate product improvements, new capabilities, and important changes in business-user-friendly language.

Audience: End users (category managers, procurement analysts, finance operators, not technical folks)

Tone: Conversational, benefit-focused, jargon-free

Structure:
```
RELEASE NOTES v[VERSION] - [QUARTER/MONTH YEAR]
===============================================

HEADLINE: [What's new? What changed?]

NEW: [Feature name] - [1-sentence description of benefit]
Description: 2-3 sentences explaining what it does and why it matters.
Impact: Who is this for? (category managers, procurement analysts, finance ops)
Example usage: Brief real-world scenario showing the benefit.

IMPROVED: [Enhancement name] - [1-sentence description of what got faster/easier]
What changed: What was slow/hard before?
Benefit: How is it better now?
Example: "Invoice matching now runs in 2 seconds instead of 15 seconds on large batches"

FIXED: [Bug name] - [1-sentence description of what no longer happens]
Issue: What was broken?
Fix: What did we fix?
Workaround: If they were affected before, what should they do now?

DEPRECATED: [Feature name] - [Will be removed on DATE]
What's changing: Feature X is being retired
Why: Why are we removing it? (replaced by feature Y, low usage, better approach)
Migration: How should customers transition?
Timeline: When does it go away?

AVAILABLE STARTING: [Date]
How to access: Where do they turn this on? (Settings > Advanced Features)
Documentation: Link to help center article
Questions: Who should they contact?

KNOWN ISSUES
Issue: [Issue name]
Impact: Which customers/scenarios are affected?
Workaround: What should they do until it's fixed?
ETA: When will we fix it?

SECURITY & COMPLIANCE UPDATES
[New certification, audit trail feature, data residency option]

---

CUSTOMER-FACING EXAMPLE: Quarterly Release Notes

RELEASE NOTES v7.4 - Q2 2026
===========================

NEW: Spend Analytics - AI-Powered Savings Discovery
Your procurement team now has an AI assistant identifying supplier pricing anomalies 
and hidden savings opportunities. Every invoice is scored for unusual patterns 
(duplicate payments, price variance, vendor consolidation opportunities). Your team 
reviews high-priority exceptions instead of manually analyzing thousands of line 
items. Early customers report 3-4% additional category savings discovery.

Impact: Category managers, chief procurement officers
Access: Go to Analytics > Spend Analytics (available for all Tier 2+ customers)
Documentation: Spend Analytics Getting Started Guide

Example: A manufacturing customer discovered $2.3M in duplicate vendor invoices 
within the first week of using Spend Analytics—payments that had already been made 
and would have gone undetected in manual review.

NEW: Purchase Requisition Mobile App
Submit, approve, and route purchase requisitions on your phone. Perfect for 
on-the-go approvals and emergency buys.

Impact: Procurement approvers, requisition submitters
Access: Download from App Store or Google Play (search "Our Company Procurement")
Documentation: Mobile app quick start

IMPROVED: Invoice Matching Performance
Invoice matching is now 60% faster on large batches (1000+ invoices). What took 
15 seconds now takes 6 seconds.

Impact: AP teams, large-volume processors
Benefit: Faster invoice clearing, quicker reporting, better cash flow visibility

IMPROVED: Compliance Dashboard
New unified dashboard showing all compliance status (SOC 2, HIPAA, FedRAMP status, 
audit trail completeness). One-click evidence export for audits.

Impact: Finance ops, compliance officers
Benefit: Audit preparation time cut from weeks to hours

DEPRECATED: Legacy Vendor Master Format
Support for the old XML vendor master import is being retired on June 30, 2026. 
We've migrated 98% of customers to our new CSV format which is faster and more 
flexible. If you're still on XML, see Migration Guide.

Why: The new format supports more fields, imports 5x faster, and is easier to 
maintain. XML support is low-volume.

Migration: See Vendor Master Migration Guide (3-step process, 15 minutes)

FIXED: Dashboard Timezone Display
Dashboards now correctly display report dates in your local timezone instead of 
UTC. (Previously, a report dated "April 10" in PT was showing as "April 11" UTC.)

Workaround: If you built reports relying on UTC timestamps before, verify your 
date filters still work correctly. Contact support if issues.

BREAKING CHANGE: RFQ REST API v1 Deprecated
REST API v1 for RFQ endpoints is being sunset on December 31, 2026. All new 
integrations must use REST API v2 (released March 2025). Existing v1 integrations 
continue working until December.

Why: v2 has better performance, more functionality, improved error handling.

Migration: See API v1 → v2 Migration Guide. Average migration: 4 hours developer time.

Action required: Integrations still on v1 will stop working January 1, 2027.

SECURITY UPDATES
✓ SOC 2 Type II Certification Complete
We've achieved SOC 2 Type II certification, auditing all controls for design and 
operating effectiveness. This covers availability, security, processing integrity, 
confidentiality, and privacy. You can download the trust report from your account.

✓ EU Data Residency Option Now Available
Customer data in the EU can now remain in EU data centers (Ireland). Select this 
option in Account Settings > Data Residency.

KNOWN ISSUES
Issue: Bulk vendor upload occasionally times out with 10,000+ records
Workaround: Upload in batches (5,000 records per upload)
ETA: Fixed in v7.5 (May 2026)

Issue: Approval workflow notifications not triggering for delegated approvers
Workaround: Approver should log in to see pending approvals (notifications coming in hotfix)
ETA: Hotfix v7.4.1 (April 25, 2026)

Questions or issues? Contact your success manager or support@company.com
```

**Template 2: Internal Engineering Release Notes (Commit-level detail)**

Purpose: Help engineering teams understand what changed, review commits, and debug issues.

Audience: Engineers, QA, technical architects

Tone: Technical, precise, implementation-focused

Structure:
```
INTERNAL RELEASE NOTES v7.4 - Q2 2026
====================================

ARCHITECTURE CHANGES
[If APIs changed, data models changed, database schema changed, new microservices added]

Database Schema Changes
- Vendor table: Added 'discount_percentage' column (numeric, nullable)
- Invoice table: Renamed 'is_matched' to 'match_status' (enum: UNMATCHED, 
  AUTO_MATCHED, MANUAL_MATCHED, EXCEPTION)
  Migration script: db/migrations/v7.4_invoice_status_migration.sql
  Impact: Custom reports using 'is_matched' will break. See migration guide.

API Changes
- GET /api/v2/invoices: New query param 'include_audit_trail' (boolean, default false)
- POST /api/v2/vendors: Vendor code now required (was optional). Max length 20.
  This breaks existing integrations sending no vendor code.
- DEPRECATED: GET /api/v1/rfq (sunset date: Dec 31, 2026)
- NEW: GET /api/v2/rfq/summary?status=OPEN (10x faster than v1 alternative)

Microservices
- New service: spend-analytics-api (Port 8082, requires ML service online)
- spend-analytics-api depends on: ml-inference (service), postgres (v12+)
- Deployment: See deployment guide for Docker Compose updates

Feature Flags
- flag: SPEND_ANALYTICS_ENABLED (controls API access, UI visibility)
- flag: NEW_INVOICE_STATUS_MODEL (controls database migration timing)
- flag: LEGACY_XML_IMPORT (controls old import code, deprecated)

COMMIT LOG (High-level summary of key commits)
[Link to commits: github.com/company/product/compare/v7.3...v7.4]

Key commits:
- a3f4d2c: Add spend-analytics-api microservice (#2341)
- b8e1c9d: Migrate invoice status to enum (#2298)
- c2d5f3a: Performance optimization: invoice matching index (#2187)

PERFORMANCE IMPROVEMENTS
- Invoice matching on 10K+ batch: 60% faster (15s → 6s)
  Root cause: Added database index on (vendor_id, invoice_date)
  Benchmark: See performance-testing/benchmarks/v7.4_results.json

- API endpoint /api/v2/invoices: 40% faster
  Optimization: Lazy-load approval chains (only if explicitly requested)
  Benchmark: 2ms (was 3.5ms) on typical 100-record response

SECURITY & COMPLIANCE
- Implemented AES-256 encryption for vendor data at rest
- Added complete audit trail to vendor create/update/delete operations
- GDPR: Added right-to-deletion endpoint (/api/v2/customers/{id}/gdpr-delete)
  Risk assessment: See /security/gdpr-impact-assessment.md

KNOWN ISSUES & LIMITATIONS
- Spend Analytics model accuracy is 85% on first deployment. Improves to 92%+ 
  after 30 days of customer data. See ml-models/accuracy-report.md
- Mobile app not yet updated for new invoice status enum (v1 mobile app still 
  expects boolean field). Will be fixed in mobile app v3.2 (end of month).
- XML vendor import deprecated. Legacy code removed in v8.0. See docs.
- RFQ API v1 sunset Dec 31, 2026. All integrations must migrate to v2.

DEPENDENCIES ADDED/UPDATED
- ml-inference: upgraded to v2.2 (new vendor anomaly detection model)
- postgres: now requires v12+ (was v11+)
- redis: added for spend-analytics caching

DEPLOYMENT NOTES
- Database migrations required (see db/migrations/)
- New environment variable required: ML_INFERENCE_HOST=http://ml-inference:8082
- New microservice to deploy: docker-compose up spend-analytics-api
- Backward compatibility: v7.4 supports v7.3 data format during migration window

TESTING COVERAGE
- Unit tests: +250 new tests (spend-analytics module) = 87% coverage
- Integration tests: +45 new tests = 92% coverage
- Spec coverage: Invoice Status Migration test suite, see tests/invoice-status/

ROLLBACK PLAN
If critical issues emerge:
1. Feature flag SPEND_ANALYTICS_ENABLED → OFF (disables new service)
2. Feature flag NEW_INVOICE_STATUS_MODEL → OFF (keeps old schema)
3. If database corruption suspected: restore from backup, redeploy v7.3
4. Note: Vendor discount_percentage column addition is backward compatible 
   (unused if SPEND_ANALYTICS disabled)
```

**Template 3: Partner/ISV Release Notes (Integration-focused)**

Purpose: Help partners understand how changes impact their integrations.

Audience: Integration partners, ISVs, system integrators

Tone: Technical, partnership-focused, impact-aware

Structure:
```
PARTNER RELEASE NOTES v7.4 - Q2 2026
===================================

FOR INTEGRATION PARTNERS
Most integrations unaffected. Three changes impact partner workflows:

1. INVOICE_MATCH_STATUS change (BREAKING for some integrations)
What changed: Boolean field 'is_matched' is now enum 'match_status' with values:
  - UNMATCHED: 0% match
  - AUTO_MATCHED: System matched automatically (>95% confidence)
  - MANUAL_MATCHED: Human reviewed and approved
  - EXCEPTION: Flagged for human review, not yet resolved

Why: Gives partners better visibility into match confidence and requires more 
granular approval workflows.

Impact: Any integration reading 'is_matched' field will break
Affected partners: [List if known, e.g., "Salesforce integration, NetSuite integration"]

Migration: See API Migration Guide
- Change: if (invoice.is_matched) → if (invoice.match_status != 'UNMATCHED')
- Timeline: 90 days (until June 30, 2026). After that, is_matched field returns null.
- Testing: Test environment live now (api-test.company.com)

2. NEW: Spend Analytics API (Optional integration, unlocks new capabilities)
We've released a new spend-analytics API that partners can integrate with. It provides:
- Supplier anomaly detection
- Savings opportunity scoring
- Vendor consolidation recommendations

This is optional—existing integrations work without it. But it's valuable if your 
customers want AI-driven spend insights.

API: GET /api/v2/analytics/spend-summary?category=supplies
Documentation: See Partner API Docs > Spend Analytics
Example: We've updated the Salesforce integration to surface spend analytics 
  insights. See github.com/company/salesforce-connector (open source)

3. PERFORMANCE: Invoice matching now 60% faster
This is great news for your integrations. Batch invoice matching requests should 
complete faster.

Old behavior: POST /api/v2/invoices/batch (10K records) → 15 seconds
New behavior: POST /api/v2/invoices/batch (10K records) → 6 seconds

No code changes required on partner side. Your integrations automatically benefit.

API VERSION LIFECYCLE
v1 RFQ API: Sunset Dec 31, 2026. If you use it, migrate to v2 now.
  Migration time: 4-6 hours developer time.
  v2 is 3x faster, supports more fields.

v2 Invoices API: Full support. This is current, will continue for 3+ years.
v2 Spend Analytics API: New in v7.4. Recommended for forward-looking integrations.

PARTNER RESOURCES
- API Migration Guide: docs.company.com/partners/api-migration-guide
- v7.4 Sandbox Available: api-sandbox.company.com (same as production)
- Partner Slack Channel: #product-releases for questions
- Webinar: "v7.4 Partner Integration Walkthrough" May 15, 2026

SUPPORT
Questions? Reach out to partner-support@company.com (SLA: 4 hours for critical issues)
```

**Template 4: Sales Team Release Notes (Deal-enabling)**

Purpose: Help sales team position changes in customer conversations and sales cycles.

Audience: Sales reps, sales engineers, sales managers

Tone: Business-focused, competitive-aware, deal-relevant

Structure:
```
SALES RELEASE NOTES v7.4 - Q2 2026
=================================

WHAT TO SAY TO CUSTOMERS

New Capabilities to Sell
1. Spend Analytics (NEW)
   Position to: CFO, VP Procurement, Category Managers
   Value: "Automatically identify 3-4% in hidden category savings"
   ROI: "Typical customer finds $1.5M in opportunities in first 90 days"
   Proof point: "Manufacturing customer identified $12M in duplicate invoices in week 1"
   Competitive advantage vs. Coupa: "They have reporting. We have AI that finds anomalies."
   Competitive advantage vs. Jaggr: "We're integrated into your sourcing workflow. They're separate."
   Deal impact: $200K+ new deal size / $50K+ expansion to existing customers

2. Mobile App (NEW)
   Position to: Busy approvers, field procurement, on-the-go operations
   Value: "Approve purchase requisitions from anywhere"
   Use case: "Traveling executive can approve urgent buys in 30 seconds"
   Competitive advantage: "Coupa mobile is basic. Ours is full-featured."
   Deal impact: Lower adoption friction, faster user satisfaction

3. Invoice Matching Performance (IMPROVEMENT)
   Position to: AP teams, finance ops, large-volume processors
   Value: "6-second invoice matching (was 15 seconds). 60% faster."
   Use case: "Can now process month-end close 1 day earlier"
   Competitive advantage: "Speed = competitive advantage in fast-moving procurement"
   Deal impact: Upsell to process-efficiency-focused prospects

4. Compliance Dashboard (IMPROVEMENT)
   Position to: Finance, Risk, Audit, Compliance officers
   Value: "Cut audit preparation time from weeks to hours"
   Use case: "SOC 2 audit coming in Q3? Dashboard shows 100% compliance status in real-time"
   Competitive advantage: "Compliance is usually a pain point. We've automated it."
   Deal impact: Win compliance-sensitive deals vs. Coupa/Jaggr

WHAT TO SAY IN COMPETITIVE SITUATIONS

vs. Coupa in Spend Analytics Deal
"Coupa has spend visibility dashboards. We have AI that actually learns from your 
spend patterns and catches what humans miss. That's the difference between reporting 
and action. Coupa's customer told us they still need to hire consultants to analyze 
their dashboard output. We do that analysis for you."

vs. Jaggr in Spend Analytics Deal
"Jaggr is great at spend analytics. But they're separate from your sourcing, 
contracting, and invoice workflows. We're integrated. Your savings discovery 
automatically flows to category manager's negotiation planning. That integration 
is where value compounds."

vs. Legacy/Homegrown in Invoice Matching
"Your manual invoicing process works, but does it catch fraud? Do you know if 
you're paying duplicate invoices? One of our customers found $2.3M in duplicates 
they'd been missing. The cost of one 30-day trial could pay for a year of our system."

OBJECTION HANDLERS RELATED TO v7.4

"This is all new. How stable is it?"
Battlecard: "Spend Analytics is already live with 15 enterprise customers in beta. 
All features have 2-week production testing before release. Mobile app is v1.0 
but production-ready. We don't release half-baked features."

"Your API is changing. Does that mean we'll need to rebuild our integrations?"
Battlecard: "Only if you're on RFQ API v1, which is being deprecated. Most customers 
use Invoices API v2 (unchanged). We provide 90-day migration period and free 
technical support to migrate if needed. Your partner should handle this, not you."

"Compliance dashboard—is that really a big deal?"
Battlecard: "If you're subject to SOC 2 audits (which you are if you process 
customer data), yes. Currently, audit prep means IT teams hunting through logs for 
3 weeks. This dashboard gives you compliance status in one screen. It's the 
difference between audit audit chaos and audit confidence."

SALES BATTLECARD UPDATES
[Spend Analytics battlecard updated with new proof points]
[Mobile App battlecard created—new deal angle for approver-focused prospects]
[Competitive positioning vs. Jaggr updated to include Spend Analytics advantage]
[ROI calculator updated with performance improvements data]

See Sales Portal > Release v7.4 Enablement for full updated materials.

DEALS TO REACTIVATE
1. [Customer Name]: Stalled on invoice processing speed. New 60% faster 
   performance → send performance specs + ROI update
2. [Customer Name]: Was interested in audit compliance. New compliance dashboard 
   → schedule demo
3. [Customer Name]: Competitive deal vs. Jaggr. New Spend Analytics integration 
   → send competitive positioning + proof point

LAUNCH TIMELINE
- April 20: v7.4 goes live to all customers
- April 21: Sales kickoff webinar (1 hour, all reps required)
- April 21: Updated battlecards in Sales Portal
- April 28: Partner webinar (optional but recommended)
- June 30: Deadline for customers on legacy XML import to migrate (sales: identify impacted customers)
- Dec 31, 2026: RFQ API v1 sunset (sales: identify integrations affected, notify customers)

Questions? Reach out to product-marketing@company.com or join the #sales-release-notes Slack channel.
```

### Breaking Change Communication Framework

Breaking changes require proactive, multi-wave communication:

**Wave 1: Notification Email (Immediate, before release)**
- Subject: "Important: [Feature Name] Changing in April" (clear, not buried)
- Body: What's changing, why, impact to customer, action required, deadline
- Audience: All affected customers (not just technical)
- Tone: Helpful, not scary

Example:
```
Subject: April 2026: Invoice API Change – Action Required by June 30

Dear [Customer],

We're improving our invoice matching API with better performance and enhanced 
features. One change affects your integrations:

WHAT'S CHANGING
Invoice status field changes from boolean (is_matched: true/false) to enum 
(match_status: UNMATCHED, AUTO_MATCHED, MANUAL_MATCHED, EXCEPTION).

WHY
This gives your integrations better visibility into match confidence, 
supporting more sophisticated approval workflows.

YOUR ACTION REQUIRED
If your integration reads the 'is_matched' field, you'll need to update it 
to use 'match_status' instead. This is typically 4 hours of developer work.

TIMELINE
- April 20: New API available in sandbox (api-sandbox.company.com)
- June 30: Old field stops working (is_matched = null)
- After June 30: Integration won't see match status

WHAT WE'RE DOING TO HELP
- Migration guide: docs.company.com/api/v2/invoice-migration
- We'll conduct a free technical review of your integration (email support@company.com)
- Sandbox available now for testing

QUESTIONS
Contact your success manager or support@company.com.

Thanks,
[Product Team]
```

**Wave 2: Migration Guide (Technical documentation)**
- Detailed step-by-step instructions
- Code examples in common languages (Node, Python, Java, C#)
- Before/after comparisons
- Testing checklist
- Common pitfalls and how to avoid them

**Wave 3: Office Hours / Support (Hands-on help)**
- Schedule live technical support sessions
- Your engineers available for questions
- Offer free code review of customer's updated integration

**Wave 4: Enforcement Communication (As deadline approaches)**
- 30 days before: "Reminder: 30 days until [old system] stops working"
- 7 days before: "One week left"
- Day of: "Effective today: [old system] no longer supported"

### API Changelog Best Practices

```
API CHANGELOG v2.x
==================

Format: Semantic versioning (major.minor.patch)
Example: v2.4.1 (major version 2, minor release 4, patch 1)

For each change, document:
1. Endpoint/Method (GET /api/v2/invoices, POST /api/v2/vendors, etc.)
2. What changed (new field, removed field, type change, behavior change)
3. Is it breaking? (Does it break existing code?)
4. Backward compatibility (Does old code still work? For how long?)
5. Migration path (If breaking, how do customers migrate?)

Example entries:

v2.5 (April 20, 2026)
---------------------

NEW: GET /api/v2/analytics/spend-summary
Endpoint: GET /api/v2/analytics/spend-summary?category={id}&period={YYYY-MM}
Returns: {
  category: string,
  period: string,
  total_spend: number,
  supplier_count: number,
  anomalies: [{ supplier_id, reason, severity }],
  savings_opportunities: [{ opportunity, estimated_savings, effort }]
}
Breaking: No
Use case: Get AI-analyzed spend insights for category

CHANGED: GET /api/v2/invoices/{id}
Field: match_status (NEW enum: UNMATCHED, AUTO_MATCHED, MANUAL_MATCHED, EXCEPTION)
Field: is_matched (DEPRECATED, returns null after June 30, 2026)
Breaking: YES if using is_matched field
Migration: Change code from:
  if (invoice.is_matched) { ... }
To:
  if (invoice.match_status !== 'UNMATCHED') { ... }
Deadline: June 30, 2026

OPTIMIZED: GET /api/v2/invoices
Performance: 40% faster (lazy-load approval chains)
Query param (NEW): include_approvals={boolean, default: false}
Recommendation: Add include_approvals=true only if you need approval data 
  to avoid unnecessary overhead

DEPRECATED: GET /api/v1/rfq
Sunset date: December 31, 2026
Replacement: GET /api/v2/rfq/summary (3x faster, more fields)
Migration: See v1→v2 RFQ Migration Guide
Timeline: 90 days to migrate (due Dec 31, 2026)

v2.4.2 (April 5, 2026) [Patch Release - Bug Fixes]
---------------------------------------------------

FIXED: POST /api/v2/invoices/batch
Issue: Large batches (10K+) occasionally timed out
Fix: Added index on (vendor_id, invoice_date)
Impact: Non-breaking. No code changes required.

FIXED: GET /api/v2/vendors/{id}/approval-history
Issue: Timezone displayed as UTC instead of customer's local timezone
Fix: Now returns approvals in customer's configured timezone
Impact: Non-breaking if you parse dates as ISO 8601 (recommended)

v2.4 (March 15, 2026)
---------------------

NEW: POST /api/v2/invoices/{id}/challenge
Allows integration to flag an invoice as potentially fraudulent/duplicate
Request body: { reason: string, supporting_evidence: [...] }
Response: { challenge_id, status: PENDING_REVIEW }
Breaking: No (optional new endpoint)

NEW: GET /api/v2/audit-trail
Get complete audit log for compliance/SoX
Queryable by entity (vendor, invoice), date range, action type
Breaking: No (new endpoint)
Use case: Compliance, fraud investigation

CHANGED: POST /api/v2/vendors
New required field: vendor_code (max 20 chars, alphanumeric + hyphens)
Breaking: YES if creating vendors without vendor_code
Migration: vendor_code can be auto-generated from vendor name if not provided
  (use query param auto_generate_code=true in POST request)
Timeline: 90 days grace period (old format still accepted until June 30, 2026)

API Versioning Strategy:
- v1: Deprecated, sunset Dec 31, 2026
- v2: Current version, all new features here
- v3: Not planned, but v2 will evolve for 3+ years

Support:
- v2 fully supported with 24/7 SLA
- v1 supported until sunset date (no new features, security patches only)
- All breaking changes announced 60+ days in advance
```

### Enterprise-Specific Considerations

**Compliance-Relevant Changes**: If release adds/changes audit trail, data access, encryption, or regulatory features, explicitly document for security/compliance buyers:
- "This release adds complete API call audit trail (required for SOC 2 compliance)"
- "Data encryption now AES-256 at rest (was AES-128)"
- "New GDPR right-to-deletion endpoint"

**Data Model Changes**: Enterprise customers often have custom reports/dashboards. If you change data models:
- List all affected fields/tables
- Provide data migration scripts if possible
- Offer free custom report rebuild consulting

**API Contract Changes**: Breaking API changes can literally break customer integrations. Communicate early, often, with migration support:
- 60+ days notice for breaking changes
- 90-day migration window minimum
- Free technical support to migrate

## Application

### Creating Release Notes for Your Quarterly Release

1. **List all changes**: Features, improvements, fixes, deprecations, breaking changes
2. **Categorize by audience**: What matters to customers? To engineers? To partners? To sales?
3. **Write customer-facing notes first**: Simple language, benefit-focused
4. **Write technical notes**: Detailed for engineers, API specs
5. **Identify breaking changes**: Create separate migration guides
6. **Create sales positioning**: How do we sell these changes?
7. **Plan communication waves**: Notification → docs → support → enforcement
8. **Get approvals**: Product, engineering, legal, compliance (if relevant)
9. **Publish**: Release notes, API docs, blog, customer email, internal wiki

### Breaking Change Checklist

- [ ] Breaking change identified and documented
- [ ] Migration path defined and documented
- [ ] Sandbox environment has new behavior available
- [ ] Notification email drafted (not jargon-heavy)
- [ ] Migration guide with code examples created
- [ ] Support team trained on migration process
- [ ] Technical support office hours scheduled
- [ ] Deadline communicated (60+ days minimum)
- [ ] Reminder emails scheduled (30 days, 7 days, 1 day before deadline)
- [ ] Enforcement plan (what happens when old system is retired)
- [ ] Legal review (if data handling, compliance, or contract implications)

## Worked Example: Quarterly Release Notes

See full customer-facing, internal engineering, partner, and sales release notes examples in Key Concepts section.

## Common Pitfalls

### Anti-Pattern 1: Burying Breaking Changes

**What Happens**: Breaking change listed in "Improvements" section. Customer doesn't see it. Integration breaks on release date. Angry support call.

**Why It Fails**: Customers don't read everything. They scan.

**Fix**: 
- Separate "Breaking Changes" section at top
- Bold/red color for breaking changes
- Email notification separate from regular release notes

### Anti-Pattern 2: No Migration Guide

**What Happens**: "API v1 is deprecated." But no guide on how to migrate. Customer's integration team frustrated, stalled.

**Why It Fails**: "Migrate" is vague. Customers need step-by-step instructions.

**Fix**: 
- Step-by-step guide with code examples
- Before/after comparisons
- Common pitfalls and solutions
- Estimated migration time

### Anti-Pattern 3: Internal Jargon in Customer Notes

**What Happens**: "We've refactored the invoice matching algorithm to optimize the vendor clustering heuristic." Customer: "What does that even mean?"

**Why It Fails**: Customers care about benefit, not implementation.

**Fix**: 
- Translate: "Invoice matching is now 60% faster"
- Use business language, not technical
- Lead with benefit, not mechanism

### Anti-Pattern 4: Insufficient Timeline

**What Happens**: "Feature X is deprecated, no longer available." Customer finds out day 1 of release. Angry.

**Why It Fails**: Enterprises need planning time. You can't take away features suddenly.

**Fix**: 
- 60+ day notice for deprecations
- 90+ day migration window for breaking changes
- Multiple reminder communications

### Anti-Pattern 5: Not Capturing Decisions in Release Notes

**What Happens**: Six months later, someone asks "Why did we change this?" No one remembers. Tribal knowledge lost.

**Why It Fails**: Historical context disappears.

**Fix**: 
- Document "Why" for major changes
- Link to design docs, ADRs (Architecture Decision Records)
- Maintain change log in wiki for future PMs

## References

**Release Notes Best Practices**
- Notion: "How to Write Release Notes" blog post
- Reforge: "Product Communication" course
- Product School: "Effective release notes framework"

**API Documentation**
- Swagger/OpenAPI: API specification standards
- APIBlueprint: API documentation format
- Stoplight: API documentation tools

**Enterprise Compliance**
- SOC 2 Audit Trail Requirements
- GDPR Right to be Forgotten Documentation
- Healthcare/HIPAA Data Handling Guidelines

**Deprecation & Migration Strategies**
- Google API Lifecycle: Deprecation best practices
- AWS Well-Architected Framework: API versioning

---

*Last updated: 2026-04-12 | Enterprise PM Catalyst v2.0*
