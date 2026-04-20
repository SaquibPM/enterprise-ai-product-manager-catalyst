---
name: analytics-instrumentation
title: "Analytics Instrumentation for Enterprise Products"
description: "Master event taxonomy design, HEART metrics for B2B, instrumentation templates, and account-level analytics for multi-tenant systems"
author: "Product Manager Catalyst"
version: "1.0"
tags: ["delivery", "analytics", "metrics", "instrumentation", "HEART", "enterprise", "B2B"]
---

# Analytics Instrumentation for Enterprise Products

## Purpose

Analytics instrumentation bridges product strategy to data. Without proper instrumentation, PMs fly blind: they can't measure feature adoption, can't identify friction points, and can't prove product/market fit. Enterprise products add complexity: multi-tenant isolation, account-level metrics (not user-level), compliance constraints on data collection, and B2B funnel metrics (account lifecycle, not conversion funnels). This skill teaches frameworks for designing event taxonomies, applying HEART metrics to B2B products, building instrumentation plans, and avoiding common pitfalls like tracking everything (data pollution) or ignoring compliance.

## Key Concepts

### 1. Event Taxonomy Design: The Object.Action Framework

Event taxonomy is the vocabulary of your analytics. Without a standard naming convention, you get chaos: "user_opened_invoice_screen", "invoice_page_viewed", "invoice_screen_opened" (same event, three names). This breaks analysis.

**The Object.Action Framework:**

```
event_name = "{object}.{action}"
```

Examples:
- `invoice.matched` (invoice was matched to a duplicate)
- `invoice.confirmed` (user confirmed a match)
- `invoice.rejected` (user rejected a match)
- `batch.processing_started` (batch job started)
- `batch.processing_completed` (batch job completed)
- `user.permission_granted` (user granted access)
- `account.upgrade` (account upgraded to premium tier)

**Properties (Metadata About the Event):**

Each event has a set of properties describing the context:

```
invoice.matched {
  properties: {
    invoice_id: string (UUID of the invoice)
    matched_invoice_count: number (how many matches found)
    confidence_score: number (0.0 - 1.0)
    algorithm_version: string (v2.1, v3.0, etc)
    processing_time_ms: number (latency of matching)
    user_id: string (user who triggered the match)
    account_id: string (tenant_id, for multi-tenant isolation)
    source: enum ["ui_manual", "batch_job", "webhook"]
    timestamp: ISO 8601 (UTC)
  }
}
```

**Hierarchy: Event Categories**

Group events into categories for easier querying:

```
Category: Invoice Lifecycle
  - invoice.created
  - invoice.matched
  - invoice.confirmed
  - invoice.rejected
  - invoice.archived

Category: Batch Processing
  - batch.processing_started
  - batch.processing_completed
  - batch.processing_failed

Category: User Access & Permissions
  - user.login
  - user.permission_granted
  - user.permission_revoked

Category: Account Management
  - account.upgrade
  - account.downgrade
  - account.user_invited
```

**Property Types & Standards:**

| Property Name | Type | Example | Notes |
|---------------|------|---------|-------|
| `event_name` | string | "invoice.matched" | Always required, constant |
| `timestamp` | ISO 8601 | "2024-04-15T10:30:00Z" | UTC, required |
| `account_id` | string | "acme-corp-001" | Tenant ID for multi-tenant. Required for B2B. |
| `user_id` | string | "user-123abc" | Unique user identifier. Required. |
| `session_id` | string | "session-456def" | Browser session. Optional but useful for funnels. |
| `property_*` | type-specific | See below | Event-specific context |

**Standard Property Naming:**
- Boolean: `is_*, has_*, should_*` (e.g., `is_duplicate`, `has_error`, `should_notify`)
- Counts: `*_count`, `*_total` (e.g., `invoice_count`, `error_total`)
- Durations: `*_ms`, `*_seconds` (e.g., `processing_time_ms`, `response_time_seconds`)
- Enums: `*_type`, `*_source`, `*_status` (e.g., `match_type`, `error_source`, `job_status`)

### 2. HEART Metrics for B2B Products

HEART metrics (Google's framework) measure product health across five dimensions. For B2B, we adapt HEART to account-level metrics, not user-level.

**H = Happiness (User Satisfaction)**

How satisfied are users with the product?

**B2B Adaptation:**
- Measure at account level (not individual user)
- Proxy: Net Promoter Score (NPS) survey after key milestones
- In-app: "How satisfied are you with invoice matching?" (1-5 scale)
- Churn risk: Accounts with low satisfaction scores trend toward cancellation

**Metric Examples:**
- NPS Score (% promoters - % detractors): Target >= 50 for enterprise
- In-app satisfaction rating: Target >= 4.0/5.0
- Feature satisfaction: "Matching accuracy meets your needs?" (yes/no + follow-up)

**Instrumentation:**
```
account_satisfaction_survey_started {
  account_id: "acme-corp-001",
  question: "How satisfied with duplicate detection?",
  feature: "invoice.matching"
}

account_satisfaction_survey_completed {
  account_id: "acme-corp-001",
  rating: 4,
  nps_score: 8,
  feedback: "Works well, but could be faster"
}
```

**E = Engagement (Usage Frequency)**

How often are users actively using the product?

**B2B Adaptation:**
- Measure at account level: Days Active per Week (DAU/WAU), Weekly Active Accounts
- Core actions: invoice.matched, invoice.confirmed (core feature usage)
- Trend: Are accounts using the product more or less over time?

**Metric Examples:**
- Weekly Active Accounts (% of accounts using product at least once/week)
- Daily Active Invoices Matched (avg invoices matched per day per account)
- Feature Adoption Rate: % of accounts using invoice matching feature

**Instrumentation:**
```
account_daily_active {
  account_id: "acme-corp-001",
  date: "2024-04-15",
  action_count: 12,
  actions: ["invoice.matched", "invoice.confirmed", "batch.processing_completed"]
}

feature_usage_summary {
  account_id: "acme-corp-001",
  week: "2024-W16",
  feature: "invoice.matching",
  usage_count: 48,
  unique_users: 3
}
```

**A = Adoption (New Users/Accounts)**

How quickly are new users/accounts getting activated?

**B2B Adaptation:**
- Measure at account level: Time-to-Value (TTV) and account activation
- Key milestone: First successful invoice match (proves feature works)
- Activation threshold: X invoices matched OR X days active

**Metric Examples:**
- Days to First Match: Median # days from account creation to first successful match (target: <3 days)
- Activation Rate: % of new accounts reaching 10 matched invoices within 7 days (target: 75%+)
- Time-to-Value: Median time from onboarding to business value realization (target: <1 week)

**Instrumentation:**
```
account.created {
  account_id: "new-client-001",
  account_name: "CompanyXYZ",
  account_tier: "enterprise",
  created_date: "2024-04-10T10:00:00Z"
}

account.activation_milestone_reached {
  account_id: "new-client-001",
  milestone: "first_invoice_matched",
  days_to_milestone: 2,
  timestamp: "2024-04-12T14:30:00Z"
}

account.activation_completed {
  account_id: "new-client-001",
  activation_metric: "10_invoices_matched",
  days_to_activation: 3,
  activated: true
}
```

**R = Retention (Churn Prevention)**

Do accounts stay and continue paying?

**B2B Adaptation:**
- Measure at account level: account churn, expansion revenue
- Churn metric: Account canceled subscription
- Retention metric: % of accounts at start of month still active at end of month
- Expansion: Accounts upgrading tier or increasing usage

**Metric Examples:**
- Monthly Account Retention: % of accounts active this month / % active last month (target: 98%+)
- Account Churn Rate: % of accounts canceled per month (target: <2%)
- Churned Account Characteristics: Accounts that churned matched fewer invoices/week, had lower satisfaction
- NRR (Net Revenue Retention): If account grows from $10K ARR to $12K MRR = 120% NRR (expansion)

**Instrumentation:**
```
account_activity_summary {
  account_id: "acme-corp-001",
  month: "2024-04",
  active: true,
  days_active: 18,
  invoices_matched: 450,
  daily_active_users: 2.5,
  satisfaction_score: 4.2
}

account.churn_event {
  account_id: "departing-client-001",
  churn_reason: "product_fit",
  account_tenure_months: 8,
  final_nrr: 0.95,
  last_active_date: "2024-03-28"
}
```

**T = Task Success (Feature Effectiveness)**

Can users accomplish their goals with the feature?

**B2B Adaptation:**
- Measure at task level: Did user successfully complete the intended action?
- Task: "Match invoices and confirm matches"
- Success: Confidence score >= 0.90 AND user confirmed the match
- Failure: Match rejected, accuracy below threshold, false positives

**Metric Examples:**
- Invoice Matching Success Rate: % of attempted matches user confirmed (target: 95%+)
- False Positive Rate: % of user-rejected matches (false positives) / total matches (target: <2%)
- Match Confidence Distribution: Histogram of confidence scores (are we matching high-confidence or low?)
- Processing Error Rate: % of batch jobs that failed (target: <0.1%)

**Instrumentation:**
```
invoice.match_result {
  account_id: "acme-corp-001",
  invoice_id: "inv-123",
  matched_invoice_ids: ["inv-124", "inv-125"],
  confidence_score: 0.97,
  algorithm_version: "v2.1",
  user_action: "confirmed",
  task_success: true
}

invoice.false_positive {
  account_id: "acme-corp-001",
  invoice_id: "inv-456",
  matched_to: "inv-457",
  confidence_score: 0.88,
  user_action: "rejected",
  reason: "different_po_number",
  task_success: false
}
```

**HEART Dashboard Example (Weekly Review for CPO):**

```
Invoice Matching Feature — Weekly Health (Week 2024-W16)

Happiness (Account Satisfaction)
  - In-app rating (post-action): 4.3/5.0 (↑ from 4.1)
  - NPS Score: 52 (↑ from 48)
  - Churn risk accounts (low satisfaction): 1 account (was 2)

Engagement (Usage)
  - Weekly Active Accounts: 42/45 (93%)
  - Daily Active Invoices Matched: 350/day (↑ from 280)
  - Core Feature Adoption: 100% (all 45 accounts used matching)

Adoption (New Accounts)
  - New Accounts This Week: 3
  - Median Days to First Match: 2.3 days (target: <3 days) ✓
  - Activation Rate (10 matches in 7 days): 66% (target: 75%, watch this)

Retention (Churn)
  - Account Retention Rate: 97.8% (target: 98%+) ✓
  - Accounts At Risk (no activity >7 days): 2 accounts
  - Expansion: 1 account upgraded tier

Task Success (Feature Quality)
  - Matching Success Rate (user confirmed matches): 96% (target: 95%+) ✓
  - False Positive Rate: 1.8% (target: <2%, watch threshold)
  - Processing Error Rate: 0.05% (target: <0.1%) ✓
  - Avg Match Confidence Score: 0.94 (target: >0.90) ✓

Red Flags:
  - Activation rate below target (66% vs 75% target)
  - 2 accounts at churn risk (no activity >7 days, low satisfaction)

Actions:
  - Sales outreach to 2 at-risk accounts; offer concierge service
  - Investigate activation: are new accounts getting slow match results?
```

### 3. Instrumentation Plan Template

An instrumentation plan documents what to measure, why, and how.

**Template:**

```yaml
Event: invoice.matched

Trigger Condition:
  - Occurs when: User selects 2+ invoices and clicks "Find Duplicates" 
    OR batch job completes and finds matches
  - Frequency: Every time a match is detected

Properties (Data Collected):
  - invoice_id: string (UUID of primary invoice)
  - matched_invoice_ids: array (UUIDs of matched invoices)
  - confidence_score: number (0.0-1.0, matching probability)
  - algorithm_version: string (v2.1, v3.0, etc)
  - processing_time_ms: number (latency of matching)
  - source: enum [ui_manual, batch_job, webhook]
  - user_id: string (user who triggered, or "system" for batch)
  - account_id: string (tenant ID, for multi-tenant)
  - timestamp: ISO 8601 (UTC)
  - session_id: string (browser session, for funnels)

Data Type:
  - Event (point-in-time occurrence, not aggregated)
  - Dimensions: confidence_score, algorithm_version, source (for grouping)
  - Measures: count, processing_time_ms (for aggregation)

Owner:
  - PM responsible: Sarah Chen (PM for invoice matching feature)
  - Data engineer implementing: Alex Patel
  - Analyst using this data: David Kim (analytics team)

Priority:
  - Critical (core feature metric)
  - Tracks: task success (are matches being made), performance (latency)

Success Criteria:
  - Event fires on every invoice match
  - Properties are populated with correct values
  - Latency of event firing < 100ms (doesn't slow down user experience)
  - Data quality check: 100% of events have invoice_id and confidence_score

Example Event (JSON):
{
  "event_name": "invoice.matched",
  "timestamp": "2024-04-15T14:30:00Z",
  "invoice_id": "inv-12345",
  "matched_invoice_ids": ["inv-12346", "inv-12347"],
  "confidence_score": 0.97,
  "algorithm_version": "v2.1",
  "processing_time_ms": 1234,
  "source": "ui_manual",
  "user_id": "user-abc123",
  "account_id": "acme-corp-001",
  "session_id": "session-xyz789"
}

Data Pipeline:
  - Collection: JavaScript SDK fires event on client (UI) or backend logs server-side (batch job)
  - Validation: Data pipeline checks for required properties
  - Storage: BigQuery table `analytics.invoice_matched` (raw events)
  - Aggregation: Daily summary table `analytics.invoice_matched_daily` (
by account, by algorithm_version)
  - Reporting: Looker dashboard "Invoice Matching Health"

Privacy & Compliance:
  - No PII stored (no invoice amounts, vendor names)
  - Compliant with GDPR (no excessive data retention, user can request deletion)
  - No external third-party sharing (data stays in-house)
  - Data retention: 2 years (for trend analysis)
```

**Instrumentation Plan Table (Spreadsheet Format):**

| Event Name | Trigger | Properties | Owner | Priority | Notes |
|------------|---------|-----------|-------|----------|-------|
| invoice.matched | User finds matches | invoice_id, confidence_score, processing_time_ms | Sarah Chen | Critical | Core feature, track latency |
| invoice.confirmed | User confirms match | invoice_id, confidence_score, source | Sarah Chen | Critical | Task success metric |
| invoice.rejected | User rejects match | invoice_id, confidence_score, rejection_reason | Sarah Chen | High | False positive detection |
| batch.processing_started | Batch job begins | batch_id, invoice_count, account_id | Alex Patel | High | Process monitoring |
| batch.processing_completed | Batch job finishes | batch_id, total_invoices, total_matches, duration_ms | Alex Patel | High | Performance tracking |
| batch.processing_failed | Batch job errors | batch_id, error_code, error_message | Alex Patel | Critical | Error monitoring |
| account.activation_milestone | First successful match | account_id, days_since_signup, milestone_name | David Kim | High | Adoption tracking |
| user.permission_granted | User access granted | account_id, user_id, role | Security team | Medium | Compliance audit trail |

### 4. Enterprise-Specific: Tenant-Level vs User-Level Analytics

B2B products track metrics at two levels: account-level (for business value) and user-level (for UX). Most enterprise PMs get this wrong.

**Account-Level Metrics (Business Value — Most Important for B2B)**

Track aggregates per account/tenant:

```
Account metrics:
  - Weekly Active Accounts: 42/45 accounts used feature this week
  - Daily Active Invoices (per account): Acme Corp matched 45 invoices/day this week
  - Days to First Match: New accounts match their first invoice in median 2.3 days
  - Monthly Retention: 97.8% of accounts active last month still active this month
  - NRR (Net Revenue Retention): Account growth in ARR or usage
  - Churn Risk: Accounts with <1 action/week for >14 days
```

**Why This Matters:**
- Buyers care about account health (will they renew?), not individual user actions
- B2B products have multiple users per account; user-level metrics are noise
- Example: "5 users at Acme Corp tried invoicing matching" (user-level) vs "Acme Corp matched 500 invoices last week" (account-level) — second is what matters

**User-Level Metrics (UX Optimization — Secondary)**

Track individual user behaviors (for understanding UX friction):

```
User metrics:
  - Time to First Match: How long does it take a new user to match their first invoice?
  - Feature Usage per User: How many invoices does a user match per week?
  - Error Encounter Rate: % of users who encounter an error, and which error?
  - Churn at User Level: Users who were active but haven't logged in >30 days
```

**Why This Matters:**
- Detect UX friction (some users get stuck at login, matching step, etc)
- Train support teams (common error = feature gap)
- Identify power users who could do product feedback interviews

**Account-Level Analysis Example:**

```
Weekly Invoice Matching Health Report (Week 2024-W16)

Query:
  SELECT 
    account_id,
    COUNT(DISTINCT DATE(timestamp)) as days_active,
    COUNT(*) as total_matches,
    AVG(confidence_score) as avg_confidence,
    MAX(processing_time_ms) as max_latency_ms,
    MIN(processing_time_ms) as min_latency_ms
  FROM analytics.invoice_matched
  WHERE timestamp >= '2024-04-08' AND timestamp < '2024-04-15'
  GROUP BY account_id
  ORDER BY total_matches DESC

Results:
  account_id         | days_active | total_matches | avg_confidence | max_latency | min_latency
  acme-corp-001      |      6      |      450      |     0.95       |     2100    |     150
  globaltech-002     |      5      |      320      |     0.92       |     1900    |     140
  startup-003        |      3      |      85       |     0.88       |     2800    |     200
  departing-004      |      1      |      10       |     0.91       |     1500    |     160
  (shows departing-004 has low activity = churn risk)

User-Level Drill-Down for departing-004:
  user_id      | matches | days_active | last_active
  user-d4-001  |    8    |      1      | 2024-04-14
  user-d4-002  |    2    |      1      | 2024-04-14
  (both users only used once this week; at risk)

Action:
  - Sales reach out to departing-004 (low usage, potential churn)
  - Feature request: departing-004 had high latency (2800ms max) — optimize matching algorithm
```

### 5. Compliance with Data Collection Policies

Enterprise products must respect privacy regulations and customer data governance.

**GDPR Compliance:**
- Collect only necessary data (no excessive properties)
- User can request deletion of personal data within 30 days
- No PII in event properties (no email, names, invoice amounts)
- Data retention < 3 years (or customer-specified)

**SOX Compliance (Financial Systems):**
- Audit trail for all changes (immutable event log)
- User ID must be captured (who made the change)
- Timestamp must be UTC (not local time)
- Retain audit logs for 7 years

**HIPAA Compliance (Healthcare):**
- Encrypt data in transit (TLS) and at rest
- No PHI (Protected Health Information) in logs
- Access control (only authorized users can query)

**Instrumentation Example (Privacy-Respecting):**

```
GOOD: invoice.matched {
  invoice_id: "inv-123" (OK: invoice ID, not PII)
  matched_invoice_ids: ["inv-124"]
  confidence_score: 0.97
  processing_time_ms: 1234
  account_id: "acme-corp-001" (OK: account ID, not PII)
  user_id: "user-abc123" (OK: user ID, not email/name)
  timestamp: "2024-04-15T14:30:00Z"
}

BAD: invoice.matched {
  invoice_id: "inv-123"
  vendor_name: "Acme Corp" (Not needed, already in invoice)
  invoice_amount: 1500.00 (PII: financial data, store separately)
  manager_email: "jane@acme.com" (PII: personal data, remove)
  manager_phone: "555-1234" (PII: personal data, remove)
  processing_time_ms: 1234
}
```

---

## Application: Worked Example

### Scenario: Enterprise Invoice Processing Platform

**Feature:** AI-powered invoice duplicate detection and matching

**Product Goals (Connected to OKRs):**
- Reduce invoice processing time from 6 hours/week to 1 hour/week (OKR 2.1)
- Enable 80% of invoices to be auto-matched (no manual reconciliation) (OKR 2.2)
- Achieve 95%+ customer satisfaction with matching accuracy (OKR 2.3)

**Team:**
- PM: Sarah Chen (responsible for metrics strategy)
- Data Engineer: Alex Patel (implementing instrumentation)
- Analyst: David Kim (building dashboards, analyzing data)

**Complete Instrumentation Plan:**

### Event 1: Account Lifecycle

**Event: account.created**

Trigger: New account signs up
Properties: account_id, account_name, account_tier (starter/professional/enterprise), created_date, company_size, industry
Owner: Sarah Chen
Priority: High (track cohorts for retention analysis)

```json
{
  "event_name": "account.created",
  "timestamp": "2024-04-15T10:00:00Z",
  "account_id": "acme-corp-001",
  "account_name": "Acme Manufacturing Corp",
  "account_tier": "enterprise",
  "company_size": "5000+",
  "industry": "manufacturing"
}
```

### Event 2: Invoice Matching Core

**Event: invoice.matched**

Trigger: System detects duplicate invoices (user action or batch job)
Properties: invoice_id, matched_invoice_ids, confidence_score, algorithm_version, processing_time_ms, source (ui_manual/batch_job), user_id, account_id
Owner: Sarah Chen
Priority: Critical (core feature, task success metric)

```json
{
  "event_name": "invoice.matched",
  "timestamp": "2024-04-15T14:30:00Z",
  "invoice_id": "inv-12345",
  "matched_invoice_ids": ["inv-12346", "inv-12347"],
  "confidence_score": 0.97,
  "algorithm_version": "v2.1",
  "processing_time_ms": 1234,
  "source": "ui_manual",
  "user_id": "user-abc123",
  "account_id": "acme-corp-001",
  "session_id": "session-xyz789"
}
```

**Event: invoice.confirmed**

Trigger: User confirms a matched set
Properties: invoice_id, matched_invoice_ids, confidence_score, user_action (confirmed/rejected), user_id, account_id
Owner: Sarah Chen
Priority: Critical (task success, validates accuracy)

```json
{
  "event_name": "invoice.confirmed",
  "timestamp": "2024-04-15T14:31:00Z",
  "invoice_id": "inv-12345",
  "matched_invoice_ids": ["inv-12346", "inv-12347"],
  "confidence_score": 0.97,
  "user_action": "confirmed",
  "user_id": "user-abc123",
  "account_id": "acme-corp-001"
}
```

**Event: invoice.rejected**

Trigger: User rejects a match (false positive detection)
Properties: invoice_id, matched_invoice_ids, confidence_score, rejection_reason (not_a_duplicate/different_po/other), user_id, account_id
Owner: Sarah Chen
Priority: High (false positive detection, accuracy monitoring)

```json
{
  "event_name": "invoice.rejected",
  "timestamp": "2024-04-15T14:32:00Z",
  "invoice_id": "inv-12345",
  "matched_invoice_ids": ["inv-12346"],
  "confidence_score": 0.78,
  "rejection_reason": "different_po_number",
  "user_id": "user-abc123",
  "account_id": "acme-corp-001"
}
```

### Event 3: Batch Processing

**Event: batch.processing_started**

Trigger: Batch matching job begins
Properties: batch_id, invoice_count, account_id, source (daily_job/weekly_job/manual_trigger)
Owner: Alex Patel
Priority: High (performance monitoring, capacity planning)

```json
{
  "event_name": "batch.processing_started",
  "timestamp": "2024-04-15T22:00:00Z",
  "batch_id": "batch-20240415-001",
  "invoice_count": 5000,
  "account_id": "acme-corp-001",
  "source": "daily_job"
}
```

**Event: batch.processing_completed**

Trigger: Batch job finishes successfully
Properties: batch_id, total_invoices, total_matches, duration_ms, average_confidence, max_latency_ms, account_id
Owner: Alex Patel
Priority: Critical (SLA tracking, performance optimization)

```json
{
  "event_name": "batch.processing_completed",
  "timestamp": "2024-04-15T22:15:00Z",
  "batch_id": "batch-20240415-001",
  "total_invoices": 5000,
  "total_matches": 1200,
  "duration_ms": 900000,
  "average_confidence": 0.94,
  "max_latency_ms": 2100,
  "account_id": "acme-corp-001"
}
```

**Event: batch.processing_failed**

Trigger: Batch job encounters error
Properties: batch_id, invoice_count, error_code, error_message, duration_ms, account_id
Owner: Alex Patel
Priority: Critical (error monitoring, incident response)

```json
{
  "event_name": "batch.processing_failed",
  "timestamp": "2024-04-15T22:30:00Z",
  "batch_id": "batch-20240415-002",
  "invoice_count": 3000,
  "error_code": "TIMEOUT",
  "error_message": "Batch processing exceeded 30-minute SLA",
  "duration_ms": 1800500,
  "account_id": "startup-003"
}
```

### Event 4: Adoption & Activation

**Event: account.activation_milestone**

Trigger: Account reaches milestone (first match, 10 matches, etc)
Properties: account_id, milestone (first_invoice_matched, 10_invoices_matched, 100_invoices_matched), days_since_signup, reached_date
Owner: David Kim
Priority: High (adoption tracking, churn prediction)

```json
{
  "event_name": "account.activation_milestone",
  "timestamp": "2024-04-17T11:20:00Z",
  "account_id": "new-client-001",
  "milestone": "first_invoice_matched",
  "days_since_signup": 2,
  "signup_date": "2024-04-15T10:00:00Z",
  "reached_date": "2024-04-17T11:20:00Z"
}
```

**Event: account.onboarding_completed**

Trigger: Customer completes onboarding (first data upload, first match, first confirmation)
Properties: account_id, time_to_completion_days, onboarding_path (guided_tour/self_service), completed_date
Owner: David Kim
Priority: Medium (user experience, onboarding funnel)

```json
{
  "event_name": "account.onboarding_completed",
  "timestamp": "2024-04-17T14:00:00Z",
  "account_id": "new-client-001",
  "time_to_completion_days": 2,
  "onboarding_path": "guided_tour",
  "completed_date": "2024-04-17T14:00:00Z"
}
```

### Event 5: Account Health

**Event: account.weekly_summary**

Trigger: Computed every Monday for previous week
Properties: account_id, week, days_active, total_matches, unique_users, avg_confidence, churn_risk_score
Owner: David Kim
Priority: Medium (account health dashboard, churn prediction)

```json
{
  "event_name": "account.weekly_summary",
  "timestamp": "2024-04-15T09:00:00Z",
  "account_id": "acme-corp-001",
  "week": "2024-W15",
  "days_active": 5,
  "total_matches": 450,
  "unique_users": 3,
  "avg_confidence": 0.95,
  "churn_risk_score": 0.05
}
```

**Event: account.churn**

Trigger: Account cancels subscription
Properties: account_id, churn_date, account_tenure_months, churn_reason (product_fit/cost/support/other), final_nrr, last_activity_date
Owner: David Kim
Priority: Critical (retention analysis, churn prevention)

```json
{
  "event_name": "account.churn",
  "timestamp": "2024-04-20T10:00:00Z",
  "account_id": "departing-client-001",
  "churn_date": "2024-04-20T10:00:00Z",
  "account_tenure_months": 8,
  "churn_reason": "product_fit",
  "final_nrr": 0.95,
  "last_activity_date": "2024-04-14T16:00:00Z"
}
```

### Event 6: Feature Errors & Performance

**Event: feature.error**

Trigger: User encounters error in matching workflow
Properties: error_code, error_message, affected_feature, user_id, account_id, timestamp, session_id
Owner: Sarah Chen + Alex Patel
Priority: High (error tracking, debugging)

```json
{
  "event_name": "feature.error",
  "timestamp": "2024-04-15T14:35:00Z",
  "error_code": "TIMEOUT_MATCHING_API",
  "error_message": "Invoice matching API did not respond within 3 seconds",
  "affected_feature": "invoice.matching",
  "user_id": "user-def456",
  "account_id": "globaltech-002",
  "session_id": "session-abc789"
}
```

**Event: feature.performance_degradation**

Trigger: Feature performance exceeds SLA
Properties: feature, metric (latency_ms, error_rate), threshold, actual_value, account_id, timestamp
Owner: Alex Patel
Priority: High (SLA monitoring, performance optimization)

```json
{
  "event_name": "feature.performance_degradation",
  "timestamp": "2024-04-15T15:00:00Z",
  "feature": "invoice.matching",
  "metric": "latency_p95_ms",
  "threshold": 2000,
  "actual_value": 2800,
  "account_id": "startup-003",
  "batch_id": "batch-20240415-003"
}
```

### HEART Dashboard (Weekly View)

**SQL Queries Built from Instrumentation Plan:**

```sql
-- Happiness: Account Satisfaction (from in-app survey)
SELECT 
  account_id,
  AVG(satisfaction_rating) as avg_satisfaction,
  COUNT(*) as survey_responses
FROM analytics.account_satisfaction_survey
WHERE timestamp >= '2024-04-08' AND timestamp < '2024-04-15'
GROUP BY account_id
ORDER BY avg_satisfaction DESC;

-- Engagement: Weekly Active Accounts
SELECT 
  DATE_TRUNC(TIMESTAMP, WEEK) as week,
  COUNT(DISTINCT account_id) as active_accounts,
  COUNT(DISTINCT account_id) / (SELECT COUNT(DISTINCT account_id) FROM analytics.accounts_active) * 100 as pct_active
FROM analytics.invoice_matched
WHERE timestamp >= '2024-04-08' AND timestamp < '2024-04-15'
GROUP BY week;

-- Adoption: Days to First Match (for new accounts)
SELECT 
  DATE_TRUNC(TIMESTAMP, MONTH) as signup_month,
  PERCENTILE_CONT(days_since_signup, 0.5) as median_ttv,
  PERCENTILE_CONT(days_since_signup, 0.9) as p90_ttv
FROM analytics.account_activation_milestone
WHERE milestone = 'first_invoice_matched'
GROUP BY signup_month;

-- Retention: Monthly Account Churn
SELECT 
  DATE_TRUNC(TIMESTAMP, MONTH) as month,
  COUNT(DISTINCT account_id) as churned_accounts,
  ROUND(COUNT(DISTINCT account_id) / LAG(COUNT(DISTINCT account_id)) OVER (ORDER BY DATE_TRUNC(TIMESTAMP, MONTH)), 4) as churn_rate
FROM analytics.account_churn
GROUP BY month
ORDER BY month DESC;

-- Task Success: Invoice Matching Accuracy
SELECT 
  DATE_TRUNC(TIMESTAMP, WEEK) as week,
  ROUND(COUNT(CASE WHEN user_action = 'confirmed' THEN 1 END) * 100.0 / COUNT(*), 2) as success_rate,
  ROUND(COUNT(CASE WHEN rejection_reason = 'false_positive' THEN 1 END) * 100.0 / COUNT(*), 2) as false_positive_rate
FROM analytics.invoice_matched
WHERE timestamp >= '2024-04-08' AND timestamp < '2024-04-15'
GROUP BY week;
```

### Dashboard Output (Weekly Report)

```
Invoice Matching — HEART Metrics (Week 2024-W16)

HAPPINESS (Account Satisfaction)
  [Chart] Average Satisfaction by Account: 4.2/5.0 (↑ from 4.0)
  [Number] In-app Feedback Responses: 38 surveys (24 last week)
  [List] Most Satisfied Accounts: 
    - acme-corp-001: 4.8/5.0
    - globaltech-002: 4.5/5.0
    - startup-003: 3.2/5.0 ⚠ (declining satisfaction)

ENGAGEMENT (Usage Frequency)
  [Number] Weekly Active Accounts: 42/45 (93%)
  [Chart] Daily Active Invoices Matched: 350/day (↑ from 280 last week)
  [Table] Usage by Account:
    - acme-corp-001: 450 matches, 6 days active
    - globaltech-002: 320 matches, 5 days active
    - departing-004: 10 matches, 1 day active ⚠ (churn risk)

ADOPTION (New Account Activation)
  [Number] New Accounts: 3 this week
  [Chart] Days to First Match (median): 2.3 days (target: <3 days) ✓
  [Funnel] Activation Funnel:
    - Account Created: 3 accounts
    - First Data Upload: 3 (100%)
    - First Invoice Matched: 3 (100%)
    - 10 Invoices Matched: 2 (67%, target: 75%)

RETENTION (Churn Prevention)
  [Number] Monthly Account Retention: 97.8% (target: 98%) ⚠
  [List] Accounts At Risk (no activity >7 days):
    - departing-004: 7 days inactive, low satisfaction (3.2/5)
    - small-client-005: 5 days inactive, normal satisfaction (4.1/5)
  [Number] Accounts Churned: 0 this week

TASK SUCCESS (Feature Effectiveness)
  [Gauge] Matching Success Rate: 96% (target: 95%+) ✓
  [Gauge] False Positive Rate: 1.8% (target: <2%) ⚠ (close to limit)
  [Chart] Confidence Score Distribution:
    - 90-95%: 25% of matches
    - 95-99%: 72% of matches
    - >99%: 3% of matches
  [Number] Processing Error Rate: 0.05% ✓

ACTIONS FOR NEXT WEEK:
  1. Sales outreach to departing-004 (churn risk) — offer support/training
  2. Investigate false positive rate (1.8% → monitor closely, consider retraining)
  3. New product question: Why only 67% of new accounts reaching 10-match activation?
     - Is matching latency slow for new accounts?
     - Is data quality issue preventing matches?
```

---

## Common Pitfalls & Anti-Patterns

### 1. Tracking Everything ("Event Spam")

Anti-pattern: Create event for every action (button click, page view, scroll, keystroke) → pollute data warehouse with noise

Pattern: Track only meaningful events aligned to strategy (invoice.matched, invoice.confirmed, account.churn) + a few UX friction signals (feature.error, feature.timeout)

Why it matters: More data ≠ better insights. Useless events create cost (data storage, query latency) and noise obscures signal.

### 2. No Naming Convention (Event Chaos)

Anti-pattern: "user_opened_invoice_screen", "invoice_page_viewed", "invoice_screen_opened" (same event, three names)

Pattern: Enforce object.action naming: "invoice.viewed", "invoice.matched", "batch.processing_completed"

Why it matters: Inconsistent naming breaks analysis. Analysts write queries for multiple event names, miss some. Dashboards break when event name changes.

### 3. Ignoring Account-Level Metrics (B2B Mistake)

Anti-pattern: "5 users from Acme Corp used invoice matching this week" (user-level, not useful)

Pattern: "Acme Corp matched 450 invoices this week, 6 days active, 3 unique users" (account-level, actionable)

Why it matters: B2B buyers care about account health, not individual user actions. Account retention > user retention.

### 4. PII in Event Properties (Privacy Risk)

Anti-pattern: Storing user email, invoice amounts, vendor names in event properties

Pattern: Store only IDs (user_id, account_id, invoice_id); link to PII separately if needed for debugging

Why it matters: PII in logs = compliance violation (GDPR, SOX). Data breaches expose sensitive info.

### 5. No Data Quality Checks (Garbage In, Garbage Out)

Anti-pattern: Events fire without validation; 10% of events missing confidence_score property

Pattern: Data pipeline validates required properties; reject invalid events; alert if validation rate drops below 99%

Why it matters: Bad data = bad insights. One missing property breaks aggregations; dashboard shows wrong numbers.

### 6. Measuring Without Business Context (Metric Drift)

Anti-pattern: "We track 50 metrics but don't know which ones drive business value"

Pattern: Every metric maps to a business goal (OKR); HEART framework ties metrics to strategy

Why it matters: Metrics should guide decisions. Without business context, teams optimize for vanity metrics (high engagement ≠ revenue).

### 7. Not Tracking False Positives (Accuracy Blind Spot)

Anti-pattern: Only track "invoices_matched"; ignore "invoices_rejected" (false positives)

Pattern: Track both confirmed_matches and rejected_matches; compute false_positive_rate

Why it matters: High volume of false positives destroys product trust. ML accuracy is only good if users accept matches.

### 8. Forget Batch Job Metrics (Backend Blind Spot)

Anti-pattern: Only track user-facing events; miss backend batch job performance

Pattern: Instrument batch jobs (start, completion, errors, duration, latency) at the same level as UI

Why it matters: Batch processing is core feature for enterprise. Missing metrics = can't monitor SLA, scale capacity, or debug errors.

### 9. No Performance Degradation Alerts

Anti-pattern: Dashboard shows matching latency 2800ms (SLA is 2000ms) but no alert

Pattern: Instrumentation includes SLA thresholds; alert when p95 latency exceeds 2000ms; log degradation event

Why it matters: Performance is feature. Without alerts, teams don't know system is degrading until customers complain.

### 10. Retention Metrics Missing Account Characteristics

Anti-pattern: "97.8% retention" (no context about which accounts churned)

Pattern: Track "churn_reason", "account_tenure_months", "final_nrr", "last_activity_date" for churned accounts

Why it matters: Churn analysis requires context. Did they churn due to poor product fit or cost? Understanding pattern enables retention strategy.

---

## References

- "HEART: Measuring Satisfaction" by Google (Rodden et al, 2010) — HEART framework origins
- "Lean Analytics" by Alistair Croll and Benjamin Yoskovitz — metrics for B2B products
- "Measuring the Immeasurable" by Douglas Hubbard — quantifying business impact
- "Marty Cagan's 'INSPIRED'" — connecting product to metrics
- "Segment's Analytics Handbook" — event taxonomy, instrumentation best practices
- "dbt Best Practices" — data transformation and pipeline quality
- Google Analytics 4 Documentation (event-driven analytics)
- Amplitude Analytics Best Practices (multi-tenant B2B)
- GDPR Article 5 (Data Minimization) — privacy-respecting instrumentation
- SOX 404 Compliance Requirements (audit trails)
