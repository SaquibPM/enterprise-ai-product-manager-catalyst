---
title: "Acceptance Criteria for Enterprise Features"
description: "Master Given/When/Then formatting, 8 AC patterns, and enterprise-specific criteria for multi-tenant, compliance-heavy features"
author: "Product Manager Catalyst"
version: "1.0"
tags: ["delivery", "acceptance-criteria", "enterprise", "testing", "compliance"]
---

# Acceptance Criteria for Enterprise Features

## Purpose

Acceptance criteria (AC) are the contract between product and engineering. Clear AC prevent mid-sprint rework, ensure testability, and reduce the gap between what PMs intend and what engineers build. In enterprise environments, AC must address multi-tenant isolation, audit trails, backward compatibility, performance SLAs, and compliance requirements — not just happy paths. This skill teaches frameworks for writing 8-10 specific, testable acceptance criteria that cover enterprise complexities and prevent scope creep.

## Key Concepts

### 1. Given/When/Then Format: The Standard for Enterprise AC

Given/When/Then is the industry standard for writing testable, unambiguous acceptance criteria. Each criterion describes a specific scenario, user action, and expected result.

**Structure:**

```
Given [initial state/context]
When [user action or system event]
Then [expected result]
And [optional additional assertions]
```

**Why Given/When/Then Works:**
- **Given:** Sets up the precondition (what must be true before the test)
- **When:** Describes the action being tested (single, specific action)
- **Then:** Specifies the expected outcome (observable, measurable result)
- **And:** Chains additional assertions (optional, for complex scenarios)

**Example AC (Invoice Matching Feature):**

```
Given a procurement manager has 3 invoices for the same PO (PO-2024-001)
When the manager selects all 3 and clicks "Find Duplicates"
Then the system returns a match confidence score of 0.97 (97%)
And displays all 3 invoices grouped in a match set
And creates an immutable audit trail entry recording the match attempt
```

**Why This Works:**
- Testable: QA can write a test case directly from this
- Specific: No ambiguity about what "matching" means
- Observable: Confidence score, grouping, audit trail are all measurable
- Complete: Includes happy path, data, and compliance (audit trail)

### 2. Eight Essential AC Patterns for Enterprise Features

Enterprise features need more than happy-path acceptance criteria. Here are 8 critical patterns:

#### Pattern 1: Functional (Happy Path)

The core feature works as designed for the primary use case.

**Template:**
```
Given [user is in state X with data Y]
When [user performs intended action Z]
Then [expected outcome occurs]
And [related data/UI updates correctly]
```

**Example:**
```
Given a procurement manager has 10 unmatched invoices in the "pending" queue
When the manager initiates automatic duplicate detection
Then the system returns matches with confidence scores >= 0.90
And displays matched invoices grouped by detected duplicate sets
```

#### Pattern 2: Edge Cases (Boundary Conditions)

Features fail at boundaries: empty data, maximum data, null values, special characters.

**Template:**
```
Given [edge case scenario: zero items, maximum items, null values, special characters]
When [user action]
Then [system handles gracefully without error or data loss]
```

**Example:**
```
Given an invoice has empty vendor name field (null)
When the matching algorithm processes this invoice
Then the system skips vendor name comparison and uses other fields (PO, amount, date)
And logs a warning (not an error) to the system

Given a batch of 500 invoices for simultaneous duplicate detection
When the manager clicks "Find All Duplicates"
Then the system returns results within 2 seconds (p95)
And does not time out or queue requests

Given an invoice contains special characters in vendor name (e.g., "O'Reilly & Assoc.")
When the matching algorithm compares to similar invoice
Then character encoding is handled correctly and match score is accurate
```

#### Pattern 3: Performance (SLA Compliance)

Enterprise customers expect performance guarantees. AC must specify latency, throughput, and resource constraints.

**Template:**
```
Given [data volume or load scenario]
When [user action]
Then [operation completes within X seconds at p95 latency]
And [system handles Y concurrent requests without degradation]
And [no memory leaks or resource exhaustion]
```

**Example:**
```
Given 100 concurrent users initiating batch invoice matching
When each user submits a batch of 50 invoices
Then the system completes each batch within 3 seconds (p95)
And maintains 99.5% request success rate (no timeouts)
And CPU usage stays below 70% on database server
And no database connection pool exhaustion occurs

Given a daily batch job processing 50,000 invoices
When the job runs at midnight (off-peak)
Then it completes within 30 minutes
And generates a completion report showing match statistics
```

#### Pattern 4: Security (Access Control & Data Isolation)

Multi-tenant systems must prevent data leakage. AC must specify isolation boundaries.

**Template:**
```
Given [user from tenant A]
When [attempting to access/modify data from tenant B]
Then [access is denied with appropriate error message]
And [the attempt is logged for audit/security investigation]
```

**Example:**
```
Given a procurement manager from Acme Corp (tenant_id: 1001)
When the manager queries the invoice matching database directly
Then the query returns only invoices for Acme Corp (tenant_id: 1001)
And invoices from other tenants (e.g., 1002, 1003) are not visible
And no row-level data leakage occurs even if database is accessed directly

Given a user with "viewer" role (no edit permissions)
When the user attempts to modify a matched invoice set
Then the system denies the action with error: "Insufficient permissions"
And logs the unauthorized attempt to the security audit trail
And no data is modified
```

#### Pattern 5: Accessibility (WCAG 2.1 AA Compliance)

Enterprise customers include users with disabilities. AC must specify accessibility requirements.

**Template:**
```
Given [accessibility scenario: keyboard navigation, screen reader, high contrast, etc.]
When [user performs action]
Then [feature is fully accessible and compliant with WCAG 2.1 AA]
```

**Example:**
```
Given a user with visual impairment using a screen reader
When the user navigates the invoice matching results page
Then all buttons, links, and form fields have descriptive aria-labels
And headings are properly structured (h1, h2, h3)
And alt text is provided for all icons and visualizations
And color is not the only way to distinguish matched vs. unmatched invoices

Given a user navigating via keyboard only (no mouse)
When the user tabs through the invoice matching workflow
Then all interactive elements are reachable via keyboard
And the tab order is logical and matches visual flow
And focus indicators are visible and meet contrast requirements (3:1 minimum)
```

#### Pattern 6: Compliance (Audit Trail, Data Retention, Regulatory)

Enterprise features must meet compliance requirements (SOX, GDPR, HIPAA, etc.).

**Template:**
```
Given [compliance scenario: data retention, audit trail, consent, etc.]
When [action occurs]
Then [compliance requirement is met]
And [audit evidence is generated and retained]
```

**Example:**
```
Given an invoice matching decision is recorded
When a procurement manager confirms a match
Then an immutable audit trail entry is created including:
  - User ID and timestamp
  - Original and matched invoice IDs
  - Confidence score and algorithm version
  - Decision (confirmed/rejected)
And the audit entry is retained for 7 years (SOX compliance)
And the audit trail cannot be deleted or modified after creation

Given a user's personal data (email, name) in an invoice
When the matching algorithm processes the invoice
Then PII is not logged, stored in debug logs, or sent to external services
And GDPR Data Processing Agreement is followed
And user can request data deletion (right to be forgotten) within 30 days
```

#### Pattern 7: Data Migration (Backward Compatibility)

Enterprise systems often integrate with legacy systems. AC must ensure backward compatibility.

**Template:**
```
Given [old system state or data format]
When [migration or compatibility action occurs]
Then [new system state is reached without data loss]
And [old data is transformed correctly]
And [no breaking changes for existing clients]
```

**Example:**
```
Given invoices in the old schema (invoice_id, vendor_id, amount only)
When the matching feature is deployed
Then all historical invoices are migrated to new schema (added: po_number, line_items, gl_account)
And no invoice records are lost or corrupted during migration
And old API clients can still retrieve invoice data using previous schema (backward compatible)
And new API clients can access enhanced fields

Given existing API clients calling GET /invoices with old response format
When the matching feature is released (new response includes match_status, confidence_score)
Then old clients continue to work without modification (new fields are optional)
And new clients can opt-in to enhanced response via query parameter ?format=v2
```

#### Pattern 8: API Contract (Request/Response Validation)

Enterprise APIs must be precise about request/response format, error handling, and versioning.

**Template:**
```
Given [API request with specific format/parameters]
When [API is called]
Then [API returns response in specified format]
And [HTTP status code is correct for success/failure]
And [error responses include actionable error messages]
```

**Example:**
```
Given a client calls POST /invoices/match with valid JSON:
{
  "invoice_ids": [123, 124, 125],
  "confidence_threshold": 0.90,
  "batch_size": 50
}
When the request is submitted
Then the API returns HTTP 200 with response:
{
  "matches": [
    {"invoice_ids": [123, 124], "confidence": 0.97},
    {"invoice_ids": [125], "confidence": 0.0}
  ],
  "processing_time_ms": 1245,
  "request_id": "req-12345"
}
And processing_time_ms is < 2000 (2s SLA)

Given a client calls POST /invoices/match with invalid request:
{"invoice_ids": "not-an-array"}
When the request is submitted
Then the API returns HTTP 400 (Bad Request)
And response includes error:
{
  "error": "INVALID_REQUEST",
  "message": "invoice_ids must be an array of integers",
  "request_id": "req-12346"
}
And the error message is actionable (not "Server error")
```

### 3. Quality Rubric: Evaluating AC Quality

Not all AC are created equal. Use this rubric to assess quality before a story starts:

**Score: 1-5 (5 = excellent)**

| Dimension | 1 (Poor) | 3 (Acceptable) | 5 (Excellent) |
|-----------|----------|----------------|---------------|
| **Testability** | Vague ("should work correctly") | Observable outcomes specified | Given/When/Then format, measurable assertions, QA can write test cases directly |
| **Specificity** | Generic ("user can match invoices") | Specific user role and scenario | Specific role, data state, action, and expected outcome; edge cases included |
| **Completeness** | Only happy path | Happy path + error cases | Happy path, edge cases, security, compliance, performance, accessibility |
| **Independence** | Depends on other AC | Mostly independent | Each AC is independently testable; no dependencies on other stories |
| **Measurability** | Subjective ("should be fast") | Measurable outcomes ("< 2s") | SLA with p95 latencies, % success rates, compliance dates, accessibility standards |
| **Enterprise Relevance** | No mention of multi-tenant/compliance | Basic security mentioned | Multi-tenant isolation, audit trail, regulatory compliance, API contracts all addressed |

**Minimum Bar for "Story Ready":**
- Testability: >= 4 (QA can write tests immediately)
- Specificity: >= 4 (no ambiguity about what's being built)
- Completeness: >= 3 (includes happy path + 1-2 critical edge cases)
- Measurability: >= 4 (outcomes are observable, not subjective)

**Red Flags (Story NOT Ready):**
- AC is a feature list ("Build duplicate detection, build approval workflow, add audit logging") instead of specific criteria
- AC is missing edge cases (null values, large data sets, permissions)
- AC has no performance or security criteria for enterprise feature
- AC is untestable ("It should be intuitive", "It should work well")
- AC doesn't address multi-tenant isolation for B2B product

### 4. Common AC Patterns by Feature Type

#### For Data Processing Features (Invoice Matching, Data Validation):
- Functional (happy path with sample data)
- Edge cases (null fields, empty sets, large batches)
- Performance (latency SLA, throughput)
- Security (multi-tenant isolation)
- Audit trail (compliance, debugging)
- API contract (request/response format)

#### For User Interface Features (Buttons, Workflows, Dashboards):
- Functional (core interaction works)
- Edge cases (empty state, error state, loading state)
- Accessibility (keyboard nav, screen reader, color contrast)
- Responsive design (mobile, tablet, desktop)
- Performance (page load time, interaction latency)
- Localization (if global product)

#### For Integration Features (API Clients, Webhooks, Data Sync):
- Functional (data syncs correctly end-to-end)
- Error handling (failed requests, timeouts, retries)
- Performance (sync latency, batch throughput)
- Security (authentication, authorization, rate limiting)
- Backward compatibility (old clients still work)
- API contract (request/response schemas, versioning)

---

## Application: Worked Example

### Scenario: "AI-Powered Duplicate Invoice Detection" for Enterprise Procurement

**Feature Overview:**
- Enterprise customers (B2B SaaS) want to automatically detect duplicate invoices
- Users: Procurement managers (100+ users per customer)
- Scale: Batch processing 10K+ invoices/week
- Compliance: SOX audit trail, GDPR data retention
- Data: Multi-tenant (15+ customers, strict isolation required)

**Story:** "As a procurement manager, I want to select invoices and identify duplicates automatically, so I can reduce manual reconciliation from 6 hours/week to 1 hour/week"

**AC Set: 12 Comprehensive Acceptance Criteria**

**AC 1: Functional - Happy Path (Single Invoice Pair)**

```
Given a procurement manager has 2 invoices:
  - Invoice A: PO-2024-001, vendor "Acme Corp", amount $1000, date 2024-04-01
  - Invoice B: PO-2024-001, vendor "Acme Corp", amount $1000, date 2024-04-02
When the manager selects both invoices and clicks "Find Duplicates"
Then the system returns match result:
  - Match confidence: 0.97 (97%)
  - Status: "High confidence duplicate"
And the invoices are displayed in a grouped match result
And the result is displayed within 1.5 seconds (p95)
```

**AC 2: Functional - Happy Path (Batch Matching)**

```
Given a procurement manager has 50 unmatched invoices queued for processing
When the manager clicks "Batch Match All"
Then the system compares all invoices pairwise
And returns match groups where confidence >= 0.90:
  - Group 1: Invoices [123, 124, 125] (97% confidence)
  - Group 2: Invoices [200, 201] (93% confidence)
  - Unmatched: Invoices [300, 301, ...] (no matches found)
And results are displayed within 2 seconds (p95)
And a batch processing ID (e.g., batch-12345) is generated for audit trail
```

**AC 3: Edge Case - Null/Missing Fields**

```
Given an invoice has missing required fields:
  - Invoice C: PO-2024-002, vendor: (null), amount: $1500, date: 2024-04-05
When the matching algorithm processes this invoice
Then the system:
  - Does NOT fail or throw an error
  - Skips vendor name comparison
  - Compares using available fields (PO, amount, date)
  - Returns match confidence based on matching fields (e.g., 0.85 for PO+amount match)
And a warning is logged: "Warning: vendor field null for invoice_id 999" (not an error)
And the invoice is still included in match results with reduced confidence
```

**AC 4: Edge Case - Large Batch (Performance Boundary)**

```
Given a user initiates batch matching for 1000 invoices
When the batch processing starts
Then the system:
  - Processes the batch without timeouts or memory errors
  - Completes within 10 seconds (p95)
  - Returns all match results grouped by confidence
  - Does NOT create duplicate or partial results if request is retried
And processing metrics are logged:
  - Total invoices processed: 1000
  - Total matches found: N
  - Processing time: X ms
  - Confidence distribution: histogram of match scores
```

**AC 5: Edge Case - No Duplicates Found**

```
Given a procurement manager processes 50 invoices with no true duplicates
When the matching algorithm runs
Then the system:
  - Returns empty match groups list (not null/error)
  - Displays message: "No duplicates detected in this batch"
  - Does NOT falsely create low-confidence matches to appear "helpful"
And the UI gracefully handles empty results (no broken layout)
```

**AC 6: Multi-Tenant Isolation (Security)**

```
Given a procurement manager from Acme Corp (tenant_id: 1001)
When the manager initiates batch matching
Then the matching algorithm processes ONLY Acme Corp invoices (tenant_id: 1001)
And invoices from other tenants (e.g., CompanyXYZ with tenant_id: 1002) are:
  - NOT visible to Acme Corp manager
  - NOT compared with Acme Corp invoices
  - NOT leaked in match results or logs
And if a user attempts to query invoices with crafted tenant_id parameter (e.g., 1002)
Then the API returns HTTP 403 Forbidden
And an unauthorized access attempt is logged to security audit trail
```

**AC 7: Row-Level Access Control (Security)**

```
Given a user with "viewer" role (read-only permissions)
When the user attempts to confirm a matched invoice set in the UI
Then the "Confirm Match" button is disabled/hidden
And the system displays message: "You don't have permission to confirm matches"
And if a viewer attempts to call PATCH /matches/123/confirm via API
Then the API returns HTTP 403 Forbidden
And the unauthorized action is logged with:
  - User ID
  - Timestamp
  - Action attempted (confirm match)
  - Reason (insufficient permissions)
```

**AC 8: Audit Trail (Compliance - SOX, GDPR)**

```
Given a procurement manager confirms a matched invoice set
When the match is confirmed
Then an immutable audit trail record is created with:
  - audit_id: UUID (immutable identifier)
  - timestamp: 2024-04-15T10:30:00Z (UTC)
  - user_id: user-123
  - user_email: jane@acme.com
  - action: "match_confirmed"
  - matched_invoice_ids: [123, 124, 125]
  - confidence_score: 0.97
  - algorithm_version: "v2.1"
  - tenant_id: 1001 (for verification during audit)
And the audit record is:
  - Written to immutable audit log (cannot be deleted or modified)
  - Retained for 7 years (SOX compliance requirement)
  - Queryable by compliance team: GET /audit-logs?user_id=123&action=match_confirmed
And no PII (invoice details, amounts) is stored in audit log, only IDs and decision
```

**AC 9: Data Retention & Privacy (GDPR Compliance)**

```
Given a user's personal data appears in an invoice (e.g., email in "ship to" field)
When the matching algorithm processes the invoice
Then:
  - Personal data is NOT logged to console or debug logs
  - Personal data is NOT sent to external analytics services
  - Personal data is NOT stored in match result metadata
And if a user requests deletion (GDPR "right to be forgotten")
When a data deletion request is submitted for user_id: X
Then the system:
  - Deletes all PII associated with that user within 30 days
  - Retains only necessary audit trail records (user_id, not name/email)
  - Confirms deletion via email
  - Logs the deletion request to compliance audit trail
```

**AC 10: Performance SLA (Latency & Throughput)**

```
Given 100 concurrent users, each matching 50-invoice batches simultaneously
When all requests arrive within a 5-second window
Then the system:
  - Completes 99% of requests within 3 seconds (p99 latency)
  - Completes 95% of requests within 2 seconds (p95 latency)
  - Maintains <1% error rate (no timeouts or failures)
  - Database connection pool does NOT exhaust (stays <80% utilization)
  - CPU on matching service stays <70%
And if load increases beyond capacity, the system:
  - Returns HTTP 429 (Too Many Requests) with retry-after header
  - Does NOT drop requests silently
  - Queues requests and processes them fairly (FIFO)
```

**AC 11: API Contract (Request/Response Format)**

```
Given a client submits a POST request to /api/v2/invoices/match:
{
  "invoice_ids": [123, 124, 125],
  "confidence_threshold": 0.90,
  "return_metadata": true,
  "batch_id_prefix": "batch-2024-q2"
}
When the request is valid
Then the API returns HTTP 200 with response:
{
  "request_id": "req-abc123def456",
  "batch_processing_id": "batch-2024-q2-789",
  "status": "completed",
  "processing_time_ms": 1245,
  "matches": [
    {
      "match_id": "match-123",
      "invoice_ids": [123, 124],
      "confidence_score": 0.97,
      "algorithm_version": "v2.1",
      "metadata": {
        "primary_invoice_id": 123,
        "duplicate_count": 1,
        "match_fields": ["po_number", "amount", "date"]
      }
    }
  ],
  "statistics": {
    "total_invoices_processed": 3,
    "total_matches_found": 1,
    "confidence_distribution": {
      "0.90-0.95": 0,
      "0.95-1.00": 1
    }
  }
}
And all fields match schema and data types
And request_id is unique and queryable for debugging
```

**AC 12: Backward Compatibility & API Versioning**

```
Given existing API clients calling POST /api/v1/invoices/match (old version)
When the v2 API is released (new version with enhanced fields)
Then:
  - Old v1 clients continue to work without modification
  - v1 responses remain unchanged (new fields not added)
  - v2 clients can request enhanced response via headers: Accept: application/vnd.api.v2+json
  - v2 response includes new fields (algorithm_version, processing_time_ms)
  - v1 and v2 can run simultaneously (no hard cutover)
  - Deprecation timeline is communicated 6 months in advance
And database migrations are backward compatible (no breaking schema changes)
```

**AC Quality Assessment (Using Rubric):**

| Dimension | Score | Notes |
|-----------|-------|-------|
| Testability | 5 | All AC are in Given/When/Then format; QA can write test cases directly |
| Specificity | 5 | Specific roles, data states, expected values (confidence 0.97, 2s latency) |
| Completeness | 5 | Happy path + edge cases + security + compliance + performance + API contract |
| Independence | 5 | Each AC is independently testable; no dependencies |
| Measurability | 5 | SLAs specified (p95 <2s, 99% requests <3s), error rates quantified |
| Enterprise Relevance | 5 | Multi-tenant isolation, audit trail, GDPR/SOX compliance, API versioning all addressed |
| **Overall** | **5** | **Story is ready for sprint** |

**Estimated AC Testing Effort:**
- Functional AC (1-2): 8 hours (happy path manual testing + automation)
- Edge cases (3-5): 12 hours (data setup, boundary testing, error scenarios)
- Security (6-7): 6 hours (access control testing, isolation verification)
- Compliance (8-9): 8 hours (audit trail validation, GDPR process verification)
- Performance (10): 10 hours (load testing, latency measurement, monitoring)
- API (11-12): 6 hours (contract validation, backward compatibility testing)
- **Total QA Effort:** ~50 hours (typical for enterprise feature with 12 AC)

---

## Common Pitfalls & Anti-Patterns

### 1. AC Are Feature Lists, Not Specific Scenarios

Anti-pattern: "User can match invoices. System validates duplicates. Matches are saved to database."

Pattern: "Given a procurement manager selects 3 invoices for the same PO, When the manager clicks 'Find Duplicates', Then the system returns 0.97 confidence match score and displays grouped result within 1.5 seconds."

Why it matters: Feature lists don't tell engineers what to build. Specific scenarios are testable.

### 2. Missing Edge Cases

Anti-pattern: "Happy path only — assumes all invoices have valid vendor names, amounts, dates"

Pattern: Include AC for null fields, empty sets, boundary values (0 invoices, 10K invoices), special characters

Why it matters: Edge cases surface in production. AC should catch them before QA testing.

### 3. No Security/Compliance AC for Enterprise Features

Anti-pattern: Forgetting to specify multi-tenant isolation, audit trail, data retention in AC

Pattern: Every enterprise feature includes AC for: multi-tenant isolation (verify no cross-tenant data leakage), audit trail (immutable record of decisions), compliance (GDPR, SOX, HIPAA as applicable)

Why it matters: Security/compliance bugs are expensive to fix post-launch. Specify them upfront.

### 4. Vague Performance Criteria

Anti-pattern: "Should be fast" or "System is responsive"

Pattern: "Batch matching for 500 invoices completes within 2 seconds (p95 latency), processing 100 concurrent requests with 99% success rate"

Why it matters: "Fast" is subjective. SLAs are measurable. Engineers optimize for SLAs, not fuzzy goals.

### 5. Untestable AC

Anti-pattern: "User experience should be intuitive", "System should be reliable", "Matching should be accurate"

Pattern: "UI follows accessibility guidelines (WCAG 2.1 AA), all interactive elements have aria-labels. System processes 1000 invoices without timeout. Matching confidence >= 0.90 for known duplicates, <0.10 for non-duplicates."

Why it matters: Untestable AC can't be verified. QA will make up their own tests or skip them.

### 6. AC with Dependencies on Other Stories

Anti-pattern: "After story 5 is done, users can confirm matches" (AC depends on another story)

Pattern: Each AC is independently testable. If feature has dependencies, state them separately in story description (not in AC).

Why it matters: AC should not create hidden sprint dependencies. Dependencies should be explicit in story blockers.

### 7. Missing API Contract for Integration Features

Anti-pattern: Forgetting to specify request/response format, error codes, status codes for APIs

Pattern: Every API-returning feature includes AC specifying: exact JSON schema, HTTP status codes (200, 400, 403, 429), error message format, request/response field types

Why it matters: API contracts prevent integration bugs. Clients depend on predictable responses.

### 8. AC for PII or Sensitive Data Without Privacy Safeguards

Anti-pattern: Storing user email, invoice amounts, PII in logs or debug output

Pattern: AC explicitly states what data is NOT logged, how PII is handled, data retention policy, GDPR compliance

Why it matters: Data privacy breaches are costly and reputation-damaging. AC should enforce privacy by design.

### 9. Not Accounting for Multi-Tenant Isolation in B2B Features

Anti-pattern: "Users can match invoices" (no mention of which tenant's invoices)

Pattern: "User from Acme Corp can only match Acme invoices; CompanyXYZ invoices are never visible or compared"

Why it matters: Multi-tenant data leakage is a critical security bug. Must be specified in AC and tested.

### 10. Acceptance Criteria Written in Passive Voice or Too Abstract

Anti-pattern: "The matching algorithm is implemented. Duplicates are identified. Results are returned."

Pattern: "When a user selects 3 invoices and clicks 'Find Duplicates', then the system returns match results with confidence scores in < 2 seconds and displays them grouped by match set."

Why it matters: Passive voice hides who does what. Active voice (user/system actions) is clearer for testing.

---

## References

- "Acceptance Criteria: The Bridge Between Intent and Implementation" (Agile Alliance)
- BDD (Behavior-Driven Development) by Dan North — Given/When/Then origins
- WCAG 2.1 Accessibility Guidelines (W3C) — accessibility AC standards
- SOX Compliance Requirements for Audit Trails (SEC)
- GDPR Data Protection Regulation (EU)
- "User Stories Applied" by Mike Cohn — user stories and AC best practices
- "Specification by Example" by Gojko Adzic — examples-driven AC approach
- REST API Versioning Best Practices (Stripe, GitHub API documentation)
- Multi-Tenant SaaS Data Isolation Patterns (AWS, Azure documentation)
