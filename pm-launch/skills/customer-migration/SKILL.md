---
title: "Customer Migration & Phased Rollout for Enterprise"
category: "Product Launch"
description: "Master phased rollout strategies, data migration for multi-tenant SaaS, and breaking change customer communication"
difficulty: "Advanced"
skills_required: ["Project Management", "Technical Understanding", "Customer Communication"]
duration_minutes: 45
---

# Customer Migration & Phased Rollout for Enterprise

## Purpose

Migrating 100+ enterprise customers from a legacy system to a new architecture, or rolling out a breaking change that impacts all customers, is one of the highest-risk moments in a SaaS company's lifecycle. A single customer with failed data migration can escalate to legal, or cause customer churn. This skill teaches you how to design phased rollouts, build migration runbooks, define rollback plans, and communicate breaking changes in ways that keep customers confident.

## Key Concepts

### Phased Rollout Framework

Never migrate all customers simultaneously. Always use a phased approach:

**Phase 1: Canary (1-2 weeks)**
- Audience: 1-3 internal customers (if you use your own product) OR highly engaged early adopter customer
- Risk tolerance: High (if things break, small blast radius)
- Support model: Direct engineering + product team
- Success criteria: 100% operational, zero data loss, performance meets SLA
- Validation: Engineering team manually validates data integrity, performance, feature parity
- Goal: Catch obvious bugs before broader rollout

Example: If migrating to new invoice matching algorithm, run it on internal Zycus data first (millions of invoices). Validate accuracy vs. old algorithm. Check performance. Only proceed to canary customer if identical results and better performance.

**Phase 2: Early Adopter (2-3 weeks after canary)**
- Audience: 5-10 customers (volunteer early adopters who explicitly opt-in)
- Risk tolerance: Medium (known issues acceptable if workaround available)
- Support model: Dedicated CS team, daily check-ins
- Success criteria: 95%+ operational (minor issues acceptable if non-blocking)
- Validation: Customer validates their real data, workflows, business processes work
- Communication: Daily updates to customer about migration progress

Example: Select customers already using new invoice matching algorithm (in beta). Migrate them to new system. Daily calls: "Are your categories matching correctly? Is your approval workflow working? Seeing any data discrepancies?" Collect feedback and fix bugs.

**Phase 3: General Availability (2-4 weeks after early adopter)**
- Audience: Remaining customers (100+ customers)
- Risk tolerance: Low (system must be stable, data integrity guaranteed)
- Support model: Standard support team, standard SLAs
- Success criteria: 99.5%+ operational (critical issues only)
- Validation: Automated testing (not just manual), monitoring dashboards
- Communication: Regular updates, proactive notification if issues

Example: Migrate remaining 200 customers in cohorts (20-30 per day). Monitor adoption metrics, support tickets, data integrity. If issues arise, pause rollout, fix, then resume.

**Phase 4: Long-Tail Migration (Ongoing, as needed)**
- Audience: Customers who opted out, customers with technical debt, customers in specific time zones
- Risk tolerance: Varies (some may require custom migration)
- Support model: Custom support by request
- Success criteria: 100% operational by deadline (usually 90 days post-GA)
- Communication: Regular check-ins, extended support window

Example: 5 customers choose to migrate in Q3 (their preferred timing). Support them individually with migration runbook + dedicated engineer if needed.

### Migration Runbook Template

```
CUSTOMER MIGRATION RUNBOOK
==========================

Customer Name: [Company Name]
Scheduled Migration Date: [Date]
Migration Type: [Data migration / Breaking change / New architecture / Feature rollout]
Migration Owner: [PM/Eng Lead Name]
Customer Point of Contact: [Name, Role, Email, Phone]
Risk Level: [Low / Medium / High]

EXECUTIVE SUMMARY
Customer is migrating from [old system] to [new system].
Expected timeline: [X hours / days]
Expected downtime: [zero downtime / [Y] hours during [specific time window]]
Rollback plan: Available if critical issues occur
Validation plan: Automated tests + manual review by customer

PRE-MIGRATION CHECKLIST (Complete 1 week before)

Customer Communication:
- [ ] Customer confirmed migration window (date/time)
- [ ] Customer identified availability (who's on call during migration?)
- [ ] Customer communicated to team: "Possible brief service interruption [date/time]"
- [ ] Customer confirmed: Can they take outage if needed? (response time if rollback needed?)

Technical Preparation:
- [ ] Backup of customer's current data taken and verified
- [ ] Database snapshots created
- [ ] Rollback plan tested in staging (confirmed we can restore)
- [ ] Migration scripts tested on staging data (simulating customer's data)
- [ ] Performance testing completed (new system performs well on their volume)
- [ ] All feature functionality validated (nothing broken in migration)
- [ ] Monitoring/alerting updated (team knows what to watch during migration)

Customer Preparation:
- [ ] Customer has exported critical reports (pre-migration baseline)
- [ ] Customer notified to stop all active operations [X] hours before migration
- [ ] Customer cleared approval queue (invoices in flight approved or rejected)
- [ ] Customer provided post-migration validation checklist
- [ ] Customer provided contact info for questions post-migration

Team Preparation:
- [ ] Migration team assigned (typically: 1 engineer running migration, 
       1 engineer monitoring, 1 PM coordinating)
- [ ] On-call escalation identified if issues arise
- [ ] Comms plan: How team communicates during migration (Slack, call bridge, etc.)
- [ ] Customer communication channel prepared (Slack, email, phone support)

MIGRATION EXECUTION (Day-of)

T-60 min (1 hour before)
- [ ] Team joins call bridge / Slack channel
- [ ] All systems normal (no pre-existing issues)
- [ ] Customer confirms ready
- [ ] Migration engineer confirms rollback plan tested

T-30 min
- [ ] Customer stops all operations (no new invoices, approvals, etc. in flight)
- [ ] Final backup taken
- [ ] Database locked (read-only mode to prevent concurrent writes)

T-0 min (Migration Starts)
- [ ] Migration engineer begins data extraction
- [ ] Monitoring engineer watches metrics:
     * Data import rate (rows/sec)
     * CPU/memory usage
     * Database size
     * Error count
- [ ] PM coordinates communication

T+[X hours] (Typical: 2-4 hours depending on customer data volume)
- [ ] Data migration complete
- [ ] Data validation scripts run:
     * Row count verification (old system = new system)
     * Data type validation (no data corruption)
     * Referential integrity check (no orphaned records)
     * Sample data spot-check (20 random invoices match old system)
- [ ] All validations pass (if fail, rollback)
- [ ] New system brought online
- [ ] Customer can log in (read-only initially)
- [ ] Notify customer: "Migration complete, system live, validating"

T+[X hours] + 30 min (Post-Migration Validation)
- [ ] Customer spot-checks data:
     * Can they see all their invoices?
     * Can they see their vendors, POs, approvals?
     * Do their custom fields display correctly?
     * Do their reports run and show correct numbers?
- [ ] Customer confirms: "Data looks correct"
- [ ] System switched to full read-write mode
- [ ] Test approval workflow (customer approves 1-2 sample invoices)
- [ ] Confirm notifications working (customer receives approval notification)
- [ ] Customer confirms: "System fully operational"

Post-Migration (First 48 hours)

Day 0 (Post-migration):
- [ ] Monitoring dashboard active (watching for anomalies)
- [ ] Daily check-in with customer: "Any issues?"
- [ ] Support tickets monitored (escalate any migration-related tickets immediately)
- [ ] Team post-mortem: Did migration go as planned? What surprised us?

Day 1 (Next business day):
- [ ] Customer confirms system stable
- [ ] Data integrity re-confirmed (random spot checks)
- [ ] Performance confirmed (matching, approvals, reporting meet SLA)
- [ ] Customer removes any temporary workarounds
- [ ] Begin warm-down of dedicated support

Day 2 (Post-migration + 2 days):
- [ ] All green
- [ ] Return to standard support SLA
- [ ] Archive migration documentation and lessons learned

ROLLBACK PLAN

If during migration we discover critical issues:

Decision criteria for rollback:
- Data corruption detected (missing rows, duplicated rows, wrong values)
- System performance below 90% of pre-migration baseline (matching taking 10x longer)
- Customer reported critical workflows broken (approvals not working, data not displaying)
- Integration failure (Salesforce/SAP integration broken post-migration)

Rollback execution (estimated 1 hour):
1. Stop all operations on new system
2. Database snapshot restored from pre-migration backup
3. Customer reverted to old system
4. Data validation run on restored data (confirm no loss)
5. Customer confirmed: "Data intact, system operational"
6. Post-mortem: Why did migration fail? What do we fix?
7. Re-plan migration for [X days later] with fixes applied

VALIDATION CHECKLIST (Customer-Facing)

Please verify the following post-migration:

Data Validation:
- [ ] Can you log into the system?
- [ ] Can you see all your invoices from the last 12 months?
- [ ] Can you see all your vendors, POs, and approvals?
- [ ] Do your custom fields (if any) display correctly?
- [ ] Do your saved reports run and show correct numbers?
- [ ] Can you access audit trail of invoice approvals?

Workflow Validation:
- [ ] Can you create a new PO?
- [ ] Can you receive an invoice matching a PO?
- [ ] Can you approve/reject an invoice?
- [ ] Do approvers receive notifications as expected?
- [ ] Can you run a custom report on [customer's typical report]?

Integration Validation (if applicable):
- [ ] Salesforce sync: Can you see invoices in Salesforce?
- [ ] SAP integration: Can you post approved invoices to SAP GL?
- [ ] [Other integration]: Is [specific data] syncing correctly?

Performance Validation:
- [ ] Does invoice matching complete in <5 seconds? (or your typical time)
- [ ] Do reports run in <10 seconds? (or your typical time)
- [ ] Does the system feel responsive (no lag in UI)?

Issues Found:
- [ ] All issues documented with screenshots/steps to reproduce
- [ ] Any issues escalated to support immediately

Sign-off:
I confirm that [company]'s migration is complete and the system is operating normally.

Customer: _____________ Date: _______ Time: _______

ESCALATION PLAN

If issues arise during migration:

Issue: Data missing (row count mismatch between old and new)
Severity: Critical (data loss = deal-killer)
Response: Immediately rollback. Investigate root cause. Re-run migration with fix.
Escalation: Notify customer immediately. Offer 2 hours for investigation before rollback decision.

Issue: Performance degraded (matching taking 10x longer)
Severity: High (could indicate data corruption or index failure)
Response: Investigate. If unfixable in <1 hour, rollback. If fixable, apply fix + re-validate.
Escalation: Notify customer. Offer choice: rollback and try again later, or wait for fix.

Issue: Customer reports approval notifications not working
Severity: Medium (customers can still approve, just no notification)
Response: Investigate notification system. Could be data migration issue or system issue.
If simple fix, apply + validate. If complex, rollback and re-plan.
Escalation: Offer workaround: "Please manually check [system] for approvals" while we fix.

Issue: Integration (Salesforce sync) not working post-migration
Severity: Medium-High (depends on customer's critical-ness of Salesforce)
Response: Check integration credentials, data mapping. Usually simple to fix.
If unfixable in <30 min, rollback and re-plan.
Escalation: Notify customer. Offer temporary workaround: "Manual sync via [process]" if critical.

COMMUNICATION TEMPLATE

Pre-Migration Email (1 week before):
Subject: Migration Scheduled: [System] — [Date] [Time]

Hi [Customer Name],

We're excited to migrate you to our new [system] on [date] at [time] [timezone].

Timeline:
- [Time]: Migration begins
- [Time]: Expected completion
- No customer action required (we handle everything)

Downtime: Estimated [X minutes / up to 2 hours] of read-only access
If critical issues arise, we may need to rollback (1-hour process).

What to do:
1. Make sure all active approvals are completed by [date at time] 
   (stop submitting new invoices ~2 hours before migration)
2. Export any critical reports now (as a backup)
3. Confirm [customer point person] will be available [date]

Questions? Email [support email] or join our migration call [date/time/link]

Post-Migration Email (if successful):
Subject: Migration Complete! — [System] is now live

Hi [Customer Name],

Great news! Your migration completed successfully at [time]. 

What's new:
- 60% faster invoice matching
- New [feature] available
- Improved [benefit]

Next steps:
- Log in and validate your data (checklist attached)
- Let us know if you see any issues
- We'll monitor closely over the next 48 hours

You can find documentation here: [link]

Post-Migration Email (if rollback required):
Subject: Migration Issue — We rolled back to restore stability

Hi [Customer Name],

During migration, we encountered [specific issue]. To keep your system stable, 
we rolled back to your previous version (all data preserved, zero data loss).

What happened:
[Explanation in plain language, not technical jargon]

What we're doing:
[Specific fix]

Next steps:
- We'll contact you [date/time] to reschedule migration
- This time, [issue] will be [fixed/addressed]
- Thanks for your patience

We apologize for the inconvenience. [Discount / credit] applied to your account.
```

### Feature Flag Strategy for Enterprise

Use feature flags to control which customers see which features, enabling safe rollout:

```
FEATURE FLAG STRATEGY FOR MIGRATION
===================================

Flag Levels (in order of scope):
1. System-wide flag: [FEATURE_ENABLED] = true/false (all customers affected)
2. Tenant-level flag: Customer A sees new feature, Customer B doesn't
3. User-level flag: User 1 in Customer A sees new feature, User 2 doesn't
4. Percentage flag: 10% of traffic sees new feature (useful for performance testing)

Example: Rolling out new invoice matching algorithm

Week 1: Canary Phase
- INVOICE_MATCHING_V2_ENABLED = false (off for all customers)
- Enable only for: Internal company test account
- Flag type: Tenant-level
- Owner: Engineering team
- Validation: Accuracy vs. old algorithm, performance testing

Week 2-3: Early Adopter Phase
- INVOICE_MATCHING_V2_ENABLED = false (default off)
- Enable for: 5-10 specific customer tenants (who opted in)
- Flag type: Tenant-level
- Owner: CS team (coordinate daily check-ins)
- Monitoring: Accuracy metrics, performance metrics, error rate
- Validation: Customer-reported accuracy, workflow functionality

Week 4: General Availability
- INVOICE_MATCHING_V2_ENABLED = true (on for all new customers)
- INVOICE_MATCHING_V1_ENABLED = true (still on, can be toggled back)
- Roll out in waves:
  * Day 1-2: 10% of remaining customers
  * Day 3-5: 50% of remaining customers
  * Day 6-7: 100% of remaining customers
- Flag type: Tenant-level with percentage rollout
- Owner: Engineering team (monitor system-wide metrics)
- Monitoring: Adoption rate, error rate, performance vs. baseline
- Rollback: If error rate > 0.5%, pause rollout and investigate

Week 5+: Stabilization
- INVOICE_MATCHING_V1_ENABLED = false (old algorithm off)
- Complete migration to v2
- Monitor for any regressions
- V1 code retired

Benefits of flag-based rollout:
✓ Can turn off feature instantly (no code deploy)
✓ Can control which customers see which version
✓ Metrics tell you if rollout is safe
✓ Rollback is one flag flip (not database migration rollback)

Implementation:
- Flags stored in database or config service
- PM can change flags via admin dashboard (no engineering needed)
- Flags checked on every relevant code path
- Metrics dashboard shows: Which customers have flag on, what's their error rate

Anti-pattern: Flag-based rollout without clear success criteria
- Bad: "We enabled the feature for Customer A. Let's see what happens."
- Good: "We enabled feature for Customer A. Success criteria: (1) 0 data corruption, 
  (2) accuracy >98% vs. old algorithm, (3) performance within 10% of baseline. 
  Daily check-ins with customer. Proceed to Phase 2 only if all criteria met."
```

### Breaking Change Customer Communication

When you must make a breaking change (e.g., API change, data model change, removing a feature), communicate proactively:

```
BREAKING CHANGE COMMUNICATION PLAN
===================================

Issue: You're deprecating RFQ API v1 (sunset Dec 31, 2026)
Impact: Integrations using v1 will stop working

Timeline:
- 6 months before: Announcement (March 2026)
- 4 months before: Documentation + migration guides
- 2 months before: Reminder to customers on v1
- 1 month before: Escalation to customer's technical team
- 1 week before: Final warning
- Sunset date: v1 stops working

Wave 1: Announcement (March 2026 - 9 months before sunset)

Email to all customers:
Subject: Heads-up: RFQ API v1 will be retired December 31, 2026

We're retiring RFQ API v1 to focus on the improved v2 API (released March 2025). 
v1 provides limited functionality; v2 is 3x faster, supports more features.

Timeline:
- Today (March 1): This announcement
- September 1: v1 stops accepting new integrations
- December 31: v1 completely offline

Action required: Migrate to v2 if using v1. Migration usually takes 4-6 hours.

Migration guide: [Link]
Help: [Support email]
Questions: [Product manager email]

Wave 2: Detailed Communication (August 2026 - 4 months before sunset)

Email to customers on v1 API (send to both technical and business contacts):
Subject: Required Action: Migrate to RFQ API v2 by December 31

We're reaching out because our records show you're using RFQ API v1, which will 
stop working December 31, 2026.

What you need to do:
1. Review the v2 API migration guide (detailed, with code examples): [Link]
2. Estimate your migration effort (typically 4-6 developer hours): [Calculator]
3. Plan your migration timeline (should complete by November to allow buffer)
4. Schedule a technical review with our team if needed: [Calendar link]

Benefits of v2:
- 3x faster API response times
- Support for more RFQ fields and workflows
- Better error messages and debugging
- Active development (new features added regularly)

Help:
- Migration documentation: [Link]
- Code examples (Node, Python, Java): [Link]
- Video walkthrough: [YouTube link]
- Technical support: [Email/Slack]

Wave 3: Escalation (September 2026 - 3 months before sunset)

For customers still on v1 (approaching non-compliance):
Send to both technical contact AND business contact (CFO, VP):

Subject: Required: RFQ API v1 Migration - 3 months remaining

[Customer Name], we're writing because your integration still uses RFQ API v1, 
which stops working in 3 months.

What happens if you don't migrate:
- December 31: RFQ API v1 goes offline
- Your integration breaks
- You can no longer [use case dependent: "submit RFQs, retrieve quotes, etc."]
- Impact: [Estimated business impact: "disruption to procurement process"]

Action steps:
1. Assign a developer to migration (4-6 hours work)
2. Test in sandbox environment (1 week)
3. Deploy to production (before Dec 31)
4. We're here to help: [Technical support contact]

Timeline:
- October 15: Deadline for starting migration
- November 15: Deadline for completing migration
- December 31: v1 offline

If you need help, let's schedule a call: [Calendar link]

Wave 4: Final Warning (December 2026 - Days before sunset)

Subject: Urgent: RFQ API v1 stops working in 3 days - Last chance to act

[Customer Name],

RFQ API v1 goes offline on December 31, 2026 (3 days from now).

If you haven't migrated, your integration will break. Immediate actions:
1. Contact us immediately for emergency migration support
2. Prepare rollback plan (switch to manual RFQ process temporarily)
3. Schedule post-holiday migration (for early January)

Emergency support: [Phone number], [Email], [Slack]
We can help expedite migration if critical to your operations.

Wave 5: Post-Sunset Communication (January 2027 - After v1 offline)

For customers who didn't migrate:
Subject: RFQ API v1 is offline - Help us get you migrated

[Customer Name],

RFQ API v1 went offline December 31 as planned. We notice your integration 
may be affected.

How we're helping:
- Free emergency migration support (normally $5K)
- Expedited technical review (next business day)
- Extended warranty on custom integrations

To get help: [Contact info]

Migration support includes:
- Technical pair programming (engineer + your developer)
- Testing in our sandbox
- Go-live support

Let's get this resolved this week: [Calendar]

Tone guidelines:
- Announced early (9 months notice minimum, ideally 12 months)
- Helpful, not scary ("we're here to help" not "your system will break")
- Provide clear migration path with resources
- Acknowledge business impact ("this affects your procurement process")
- Escalate to both technical and business stakeholders
- Offer support (don't just tell them to figure it out)
```

## Application

### Planning Your First Phased Rollout

**Month 1: Plan**
- Define 4 phases (canary, early adopter, GA, long-tail)
- Identify 1-2 canary candidates
- Identify 5-10 early adopter candidates
- Create migration runbook
- Create monitoring dashboard

**Month 2: Canary Phase**
- Execute migration with 1-2 internal/close customers
- Validate end-to-end
- Document learnings
- Update runbook

**Month 3: Early Adopter Phase**
- Execute migrations with 5-10 customers
- Daily check-ins, gather feedback
- Document issues and fixes
- Update runbook again

**Month 4: General Availability**
- Roll out to remaining customers in waves
- Monitor adoption metrics
- Support team available
- Fix bugs as they emerge

### Migration Runbook Checklist

Use the template in Key Concepts. For your next migration, customize:
- Customer name and contact info
- Specific data validation steps (relevant to your data model)
- Specific workflow tests (relevant to your product)
- Specific integration tests (if applicable)
- Specific monitoring metrics (what indicates problems?)

## Worked Example: Migrating 200 Customers to New Invoice Matching Algorithm

**Context**: Enterprise procurement platform. Replace legacy invoice matching with new AI-powered matching. 200+ existing customers. Critical data: 50M+ invoices across all customers. Zero downtime required. Zero data loss acceptable.

### Phase 1: Canary (Week 1-2)

**Candidate**: Internal Zycus data (2M test invoices)

**Pre-Migration**:
- Data backup taken and verified
- Rollback plan tested
- Migration scripts tested on staging
- Performance baseline: Matching 2M invoices takes 4 hours
- Monitoring dashboard built: Watch CPU, memory, matching accuracy, error rate

**Execution**:
- Run migration: Extract 2M old invoices, load into new algorithm
- Validate: Row count match, spot-check 100 random invoices (accuracy vs. old algorithm)
- Performance: New matching takes 2.5 hours (37% improvement)
- Accuracy: 99.2% match rate vs. 98.1% old (improvement confirmed)

**Result**: Success. Proceed to early adopter phase.

### Phase 2: Early Adopter (Week 3-5)

**Candidates**: 5 customers who volunteered:
- Large manufacturing customer (50M invoices)
- Healthcare system (20M invoices)
- Retail customer (30M invoices)
- Tech company (8M invoices)
- Mid-market distributor (5M invoices)

**Customer 1: Large manufacturing (50M invoices)**

Runbook execution:
- Pre-migration: Data backup, rollback tested, customer confirms ready
- Migration: 50M invoices → new algorithm (estimated 8 hours for their volume)
- Validation: Row count verified (50M in, 50M out), accuracy spot-check (200 random invoices), performance baseline set
- Post-migration: Customer logs in, spot-checks: "All my invoices here? Do they match correctly?" Daily check-ins.

Day 1 post-migration: All green, customer confirms.
Day 2-3: Customer runs full reports, spot-checks matching accuracy, tests approval workflows. All working.
Day 4-5: Customer begins normal operations (new invoices flowing in). System performs well.

Result: Success. Customer reports 8% improvement in matching accuracy, faster processing.

**Customer 2-5**: Similar execution. All succeed.

### Phase 3: General Availability (Week 6-8)

**Rollout wave strategy**:
- Wave 1 (Week 6, Mon-Tue): 30 customers (largest customers first)
- Wave 2 (Week 6, Wed-Fri): 50 customers
- Wave 3 (Week 7, Mon-Wed): 60 customers
- Wave 4 (Week 7, Thu-Fri): 40 customers
- Wave 5 (Week 8): Remaining 20 customers

**Execution per wave**:
- Batch 5 customers per day (stagger to avoid concentration)
- Migration runbook: 2-4 hours per customer
- Validation: Automated (data integrity tests, performance tests)
- Monitoring: Dashboard shows: # migrated, error rate, performance variance
- Rollback: If any customer exceeds error threshold (>0.1% data loss rate), pause, investigate, roll back that customer, fix, re-migrate

**Monitoring metrics during Wave 1-2**:
- Success rate: 28/30 = 93% (2 had issues, rolled back successfully)
- Error rate: 0.01% (well below threshold)
- Performance variance: New system 35% faster on average
- Customer satisfaction: 27/30 reported positive, 2 were neutral, 1 reported issue (now resolved via rollback)

**Decision**: Continue to Wave 3-4 (all success criteria met)

### Phase 4: Long-Tail (Ongoing)

**Remaining customers** (20 not in waves):
- Some chose specific migration dates (their preferred timing)
- Some have contractual obligations (annual migrations scheduled)
- Some requested postponement

Support with:
- Standard runbook (from Phase 1-3)
- Dedicated engineer if complex (e.g., very large data volume)
- Coordination with customer for timing

**Post-Migration Stabilization**:
- Week 8-10: Monitor for regressions (error rate staying low, performance stable)
- Week 11-12: Confirm all 200 customers successfully migrated (status report)
- Ongoing: Legacy matching algorithm turned off (no longer needed)

**Results** (3 months post-GA):
- 200/200 customers successfully migrated
- 0 data loss incidents
- 35% average performance improvement
- 1-2% average accuracy improvement
- 0 escalations to legal (zero-tolerance data loss paid off)
- Customer satisfaction: 94% of customers reported "same or better" experience

## Common Pitfalls

### Anti-Pattern 1: Big-Bang Migration

**What Happens**: Migrate all customers simultaneously. 5% experience issues. Support overwhelmed. Rollback chaotic.

**Why It Fails**: You can't monitor and support 200 simultaneous migrations.

**Fix**: Phase 1-4 approach. Start small, increase scope gradually.

### Anti-Pattern 2: No Rollback Plan

**What Happens**: Customer reports data corruption. You have no way to roll back. Customer loses confidence. Escalates to legal.

**Why It Fails**: Data corruption is unforgivable. You must have instant rollback.

**Fix**: Test rollback procedure in staging. Know exactly how to restore from backup.

### Anti-Pattern 3: Insufficient Customer Communication

**What Happens**: Customer not told migration is happening. System down during their peak time. Angry call.

**Why It Fails**: Enterprise customers need advance notice and control.

**Fix**: 1-week notice minimum. Let customer pick migration window. Confirm availability.

### Anti-Pattern 4: Not Validating Data Post-Migration

**What Happens**: Data corrupts during migration. Not caught until customer reports issue weeks later. By then, fix is hard (data altered).

**Why It Fails**: Data integrity is foundational.

**Fix**: Automated validation suite: row counts, referential integrity, sample data spot-checks.

### Anti-Pattern 5: Removing Old System Too Soon

**What Happens**: Migrate all customers to new system. Turn off old system. Discover bug in new system. Can't fall back because old system is gone.

**Why It Fails**: You need safety net.

**Fix**: Keep old system running for 30 days post-GA. Only decommission after stabilization.

## References

**Phased Rollout Patterns**
- Google SRE Book: Gradual rollout strategies
- Stripe Engineering: Database migration patterns
- Figma: Zero-downtime migrations

**Feature Flag Management**
- LaunchDarkly: Feature management platform
- Unleash: Open-source feature flags
- AWS AppConfig: Feature flag service

**Customer Communication Templates**
- Notion: Customer communication playbooks
- Reforge: Product communication course
- Product School: Migration communication frameworks

**Data Migration Tools**
- Liquibase: Database migration versioning
- Flyway: Database migration automation
- AWS DMS: Database migration service

---

*Last updated: 2026-04-12 | Enterprise PM Catalyst v2.0*
