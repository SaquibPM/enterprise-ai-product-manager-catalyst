---
name: api-spec-design
description: "Design APIs for enterprise integrations. Master versioning strategy, backward compatibility, multi-tenant auth, rate limiting, and breaking change management. Stop shipping APIs that break customer integrations."
pushy: "Every breaking change to an API is a customer support incident. Every unversioned API is a technical debt timebomb. Your enterprise customers have 50+ integrations consuming your APIs. Design like you're shipping critical infrastructure."
purpose: "Build APIs that enterprise customers can reliably integrate with, extend without fear, and upgrade without breaking their systems."
---

# API Specification Design for Enterprise Integrations

## Purpose
An API is a contract. In enterprise software, that contract is legally binding. When you ship an API without versioning, you're telling customers: "We own your integration; we can break it whenever we want." When a breaking change lands in production, customers spend weeks rewriting integration code. You lose trust. You lose the deal.

This skill teaches you to design APIs like you're shipping critical infrastructure, not a feature. It covers versioning strategy, multi-tenant auth, rate limiting, backward compatibility, and the breaking change management process that keeps customer integrations alive.

## Key Concepts

### 1. The Enterprise API Mandate
Enterprise customers don't integrate with one API. They have 50+ integrations: ERPs (SAP, Oracle), business intelligence (Tableau, Looker), workflow automation (Zapier, Make), custom apps, and data lakes.

Your API is one piece of their infrastructure. If you break it, you break their entire data pipeline.

Enterprise API design must account for:
- **Versioning**: You own v1; you'll support it for 2 years. Then v2 is the new stable. Customers upgrade on their timeline, not yours.
- **Backward Compatibility**: New features should not break old code. If you must break something, deprecate it with 90-day notice.
- **Multi-Tenant Auth**: One customer's API key must not grant access to another's data. Auth is not optional.
- **Rate Limiting**: Prevent one runaway customer from taking down the system for everyone else.
- **Webhook Events**: Some customers don't want to poll; they want to be notified. Webhooks are synchronous integration done right.
- **Breaking Change Policy**: Clear, documented, with migration paths. No surprise breaking changes.

### 2. API Design Decision Tree

```
START: "I need to design an API."

Question 1: How much data will customers query?
├─ Small (< 1000 records): REST is fine
├─ Large (1M+ records) with complex filtering: Consider GraphQL or gRPC
└─ Real-time streaming: Webhooks or Server-Sent Events (SSE)

Question 2: Are updates synchronous or asynchronous?
├─ Synchronous (customer waits for response): REST POST
├─ Asynchronous (customer fires and forgets): Webhooks
└─ Both: REST POST returns job_id; customer polls /jobs/{id} for status

Question 3: Multi-tenant?
├─ Single tenant: Auth is basic
├─ Multi-tenant (all enterprise): Auth must enforce tenant boundaries
└─ Multi-org (e.g., marketplace): Scoped API keys per org

Question 4: Performance SLAs?
├─ < 100ms P95: Simple REST, optimized queries
├─ 100-500ms P95: REST with pagination
├─ > 500ms P95: Webhooks or batch APIs
└─ Real-time (<10ms): gRPC or WebSocket

DECISION:
- REST: Most use cases (list, filter, create, update, delete)
- GraphQL: Complex filtering, reduce over-fetching
- gRPC: High-performance, low-latency microservices
- Webhooks: Event-driven, async notifications
- Batch API: Large data exports (contracts, invoices, POs)
```

### 3. REST API Principles for Enterprise

#### 3a. Resource-Oriented Design
Every API resource represents a business object. Name resources by what they are, not what they do.

Good REST:
```
GET /api/v1/contracts              # List contracts
POST /api/v1/contracts             # Create contract
GET /api/v1/contracts/{id}         # Get contract
PATCH /api/v1/contracts/{id}       # Update contract
DELETE /api/v1/contracts/{id}      # Delete contract

GET /api/v1/contracts/{id}/risks   # Get risks for a contract
```

Bad REST (RPC-style):
```
GET /api/v1/getContracts           # Not resource-oriented
POST /api/v1/createContract        # Action is redundant (POST = create)
POST /api/v1/deleteContract        # Should be DELETE
GET /api/v1/analyzeContractRisk    # Not a resource
```

#### 3b. HTTP Status Codes
Use status codes semantically. Enterprise customers parse these to determine if a retry is needed.

```
200 OK:                    Request succeeded; return data
201 Created:               New resource created (used for POST)
204 No Content:            Success but no body (used for DELETE)
400 Bad Request:           Client error; customer's fault (bad input)
401 Unauthorized:          Missing/invalid auth token
403 Forbidden:             Auth token valid but customer lacks permission
404 Not Found:             Resource doesn't exist
409 Conflict:              Business logic violation (e.g., duplicate contract)
429 Too Many Requests:     Rate limit exceeded; customer must retry later
500 Internal Server Error: Server error; not customer's fault (customer should retry)
503 Service Unavailable:   Temporary outage; customer should retry with backoff
```

**Why it matters**: A customer's integration code checks the status code to decide: Should I retry? Should I wait? Should I alert ops?

If you return 500 for a business logic violation, the customer's integration keeps retrying forever.

#### 3c. Error Response Format
Standardize error responses so customers can parse them.

Good error response:
```json
{
  "error": {
    "code": "INVALID_CONTRACT_AMOUNT",
    "message": "Contract amount must be positive",
    "details": {
      "field": "amount",
      "value": -1000,
      "constraint": "amount > 0"
    }
  }
}
```

Bad error response:
```json
{
  "message": "Something went wrong"
}
```

**Why it matters**: Customer's integration code can parse the error code and respond appropriately:
- If code == "INVALID_CONTRACT_AMOUNT", alert user to fix the input
- If code == "RATE_LIMIT_EXCEEDED", back off and retry
- If code == "SUPPLIER_NOT_FOUND", fail fast; no point retrying

### 4. Versioning Strategy

#### 4a. URL Path Versioning (Recommended for Enterprise)
```
GET /api/v1/contracts
GET /api/v2/contracts  # New schema, breaking changes
```

**Pros**: Clear, explicit, easy to route, easy to sunset old versions
**Cons**: URLs are longer; looks like "API bloat"

#### 4b. Header Versioning (Not Recommended)
```
GET /api/contracts -H "Accept: application/vnd.zycus.v2+json"
```

**Pros**: Clean URLs
**Cons**: Harder to debug (version hidden in headers); easy for customer to forget header and get old behavior

#### 4c. Sunset Policy
Every major version has a sunset date.

Example:
```
API v1:    2023-01-01 to 2025-01-01 (2 years support)
API v2:    2024-06-01 to 2026-06-01 (2 years support, overlaps v1 for 6 months)
API v3:    2026-01-01 to 2028-01-01 (2 years support, overlaps v2 for 6 months)
```

**Communication Plan**:
- Month 1: Announce v2 is coming; v1 will sunset in 24 months
- Month 12: Email customers; "v1 sunsetting in 12 months; upgrade now"
- Month 20: "v1 sunsetting in 4 months; no more excuses"
- Month 23: "v1 sunsetting in 1 month; we'll help with migration"
- Month 24: v1 endpoints return 410 Gone; customer is forced to upgrade

### 5. Backward Compatibility Rules

#### Rule 1: Adding New Fields is Safe
Old clients ignore new fields; they don't break.

```json
/* v1 response */
{ "id": 1, "name": "Supplier A", "status": "active" }

/* v2 response (backward compatible) */
{ "id": 1, "name": "Supplier A", "status": "active", "riskScore": 75 }

Old client still works; ignores riskScore.
```

#### Rule 2: Removing Fields Breaks Clients
Don't remove fields from existing API versions. Ever.

If you want to stop returning a field, mark it as deprecated in v1; remove it in v2.

#### Rule 3: Changing Field Type Breaks Clients
```
/* v1 */
{ "amount": 1000 }  /* number */

/* v2 (BREAKING) */
{ "amount": "1000" } /* string */

Old client does: total += response.amount (expects number)
Now: total += "1000" (type error!)
```

#### Rule 4: Reordering JSON Keys is Safe
JSON is unordered; order doesn't matter.

```
/* v1 */
{ "id": 1, "name": "A", "status": "active" }

/* v2 (safe) */
{ "name": "A", "status": "active", "id": 1 }

Same data, different order. Old clients don't care.
```

#### Rule 5: Adding Required Request Fields Breaks Clients
If you add a required field to a request, old clients won't include it.

```
/* v1 request (old client sends) */
POST /api/v1/contracts
{ "supplierId": 1, "amount": 1000 }

/* v2 schema (NEW requirement added) */
POST /api/v2/contracts
{ "supplierId": 1, "amount": 1000, "riskLevel": "high" }  /* required */

Old client's request missing riskLevel → 400 Bad Request
```

Solution: Make new fields optional with defaults.
```
/* v2 schema (backward compatible) */
POST /api/v2/contracts
{ "supplierId": 1, "amount": 1000, "riskLevel": "medium" }  /* optional, default: "medium" */

Old client's request works; uses default.
```

### 6. Multi-Tenant Authentication & Authorization

In enterprise, API security is non-negotiable. Multi-tenant violations (one tenant accessing another's data) are GDPR violations and customer churn events.

#### 6a. API Key Structure
```
Header: Authorization: Bearer sk_live_abc123xyz789_...

Structure:
- Prefix: sk_live_ (live environment) or sk_test_ (sandbox)
- Tenant ID embedded: abc123xyz789 encodes tenant_id
- Secret: random, cryptographically secure
```

Why embedded tenant ID? When you receive a request, you can:
1. Decode tenant_id from token prefix
2. Verify the signature
3. Check: Does this tenant_id match the request scope?

If a customer's API key tries to access a different tenant, you can immediately reject it.

#### 6b. Request-Level Tenant Enforcement
```
GET /api/v1/contracts?tenant=1234

Check: Does the API key's embedded tenant match the requested tenant?
If no match, return 403 Forbidden.
```

Never allow a customer to specify `?tenant=OTHER_TENANT` and get OTHER_TENANT's data.

#### 6c. Webhook Auth
Webhooks are called FROM your server TO the customer's server. You must prove it's you.

```
POST https://customer-server.com/webhooks/contracts
Headers:
  X-Signature: sha256=HMAC-SHA256(payload, webhook_secret)
  X-Timestamp: 1234567890

Customer verifies:
1. Signature matches their stored webhook_secret
2. Timestamp is recent (within 5 min); prevents replay attacks
```

---

## Application: Step-by-Step API Design

### Step 1: Define the Resource Model
List all resources customers will interact with.

**Example: Enterprise Procurement Platform**

Resources:
```
Contracts:   Core business object; contracts between company and suppliers
Suppliers:   Master data; supplier info
Risks:       Derived; computed from contracts (one risk per contract)
Approvals:   Workflow; who approved what, when
Attachments: Documents; contract PDFs, amendments
Audit Logs:  Immutable; what changed, when, who did it
```

### Step 2: Design the REST Endpoints

For each resource, design CRUD operations:

**Contracts Resource**:
```
GET /api/v1/contracts              # List (paginated, filtered)
POST /api/v1/contracts             # Create
GET /api/v1/contracts/{id}         # Get one
PATCH /api/v1/contracts/{id}       # Update (partial)
DELETE /api/v1/contracts/{id}      # Delete (logical, not physical)
POST /api/v1/contracts/{id}/approve # Custom action: approve contract
```

### Step 3: Define Request/Response Schemas

**GET /api/v1/contracts/{id} Response**:
```json
{
  "id": "contract_abc123",
  "supplier_id": "supplier_xyz789",
  "supplier_name": "Acme Corp",
  "amount": 50000,
  "currency": "USD",
  "status": "approved",
  "risk_score": 75,
  "risk_category": "medium",
  "flagged_terms": ["liability_cap", "indemnification"],
  "start_date": "2026-01-01T00:00:00Z",
  "end_date": "2027-01-01T00:00:00Z",
  "created_at": "2026-01-01T10:30:00Z",
  "updated_at": "2026-01-02T14:15:00Z",
  "created_by": "user_john_123",
  "approved_by": "user_jane_456",
  "approved_at": "2026-01-02T14:15:00Z",
  "tenant_id": "tenant_1234"  /* Don't expose in response; for internal use only */
}
```

### Step 4: Define Query Parameters

**GET /api/v1/contracts?...** supports filtering, sorting, pagination:

```
Filtering:
  ?status=approved
  ?risk_score_min=50&risk_score_max=100
  ?supplier_id=supplier_xyz789
  ?start_date_after=2026-01-01

Sorting:
  ?sort=risk_score:desc  (highest risk first)
  ?sort=created_at:asc   (oldest first)

Pagination:
  ?limit=100&offset=0    (first 100 items)
  ?limit=100&offset=100  (items 100-200)
```

### Step 5: Design Rate Limiting

**Rate Limit Strategy for Enterprise**:
```
Tier 1 (Free/Trial): 100 requests/minute, 1000 requests/day
Tier 2 (Standard):   1000 requests/minute, 100k requests/day
Tier 3 (Enterprise): 10k requests/minute, unlimited daily

Per-tenant limits enforce fairness:
- One runaway customer can't consume all quota
- Tenant A's heavy usage doesn't impact Tenant B
```

**Rate Limit Response Headers**:
```
X-RateLimit-Limit:      1000
X-RateLimit-Remaining:  987
X-RateLimit-Reset:      1234567890
Retry-After:            60 (seconds to retry)
```

**When rate limit is exceeded**:
```
429 Too Many Requests
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "You have exceeded your rate limit of 1000 requests/minute",
    "retry_after": 60
  }
}
```

### Step 6: Design Webhooks

**Contract Status Change Event**:
```
Trigger: When contract status changes (e.g., from "pending_approval" to "approved")

Event Payload:
{
  "event_id": "evt_abc123xyz",
  "event_type": "contract.status_changed",
  "timestamp": "2026-01-02T14:15:00Z",
  "data": {
    "contract_id": "contract_abc123",
    "status_before": "pending_approval",
    "status_after": "approved",
    "approved_by": "user_jane_456",
    "approved_at": "2026-01-02T14:15:00Z"
  },
  "tenant_id": "tenant_1234"
}

Delivery:
POST https://customer-server.com/webhooks/contracts

Headers:
  X-Signature: sha256=hmac...
  X-Timestamp: 1234567890
  X-Event-Id: evt_abc123xyz

Retry policy:
- Attempt 1: Immediate
- Attempt 2: 1 hour later
- Attempt 3: 1 day later
- After 3 failures: Mark as dead; alert customer

Idempotency:
Customer should handle duplicate events (event_id is the idempotency key).
If customer receives same event_id twice, ignore the second.
```

---

## Worked Example: Supplier Data Synchronization API

### Use Case
A Fortune 500 customer has suppliers in their ERP (SAP). They want to sync supplier data to Zycus daily for contract risk scoring. Currently, they export CSV from SAP, upload manually. This is error-prone and slow.

Solution: Build an API that allows the customer's ERP to push supplier data changes to Zycus in real-time.

### API Design

**Resource: Suppliers**

**POST /api/v1/suppliers (Create or Update)**
```
Request:
{
  "external_id": "SAP-SUPPLIER-001",  /* Customer's ID in SAP */
  "name": "Acme Corp",
  "country": "US",
  "industry": "Manufacturing",
  "annual_revenue": 500000000,
  "credit_rating": "A+",
  "legal_entity_type": "Corporation",
  "payment_terms": "Net 30",
  "primary_contact": {
    "name": "John Smith",
    "email": "john@acme.com",
    "phone": "+1-555-0100"
  },
  "is_active": true
}

Response (201 Created):
{
  "id": "supplier_abc123",  /* Zycus ID */
  "external_id": "SAP-SUPPLIER-001",
  "name": "Acme Corp",
  "status": "active",
  "created_at": "2026-01-02T14:15:00Z",
  "updated_at": "2026-01-02T14:15:00Z"
}
```

**Idempotency for Safe Retries**:

If the ERP's integration crashes after sending this request but before confirming delivery, how does it know whether the supplier was created or not?

Solution: Use idempotency key.

```
Request (with idempotency key):
Headers:
  Idempotency-Key: sap-export-run-2026-01-02-14:15:00

Body:
{
  "external_id": "SAP-SUPPLIER-001",
  "name": "Acme Corp",
  ...
}

Server behavior:
- First request with this Idempotency-Key: Create supplier; store key → response mapping
- Second request with same Idempotency-Key: Return the same response (don't create again)

Result: Even if ERP retries 10 times, supplier is created once.
```

**GET /api/v1/suppliers/{id}**
```
Response:
{
  "id": "supplier_abc123",
  "external_id": "SAP-SUPPLIER-001",
  "name": "Acme Corp",
  "country": "US",
  "is_active": true,
  "created_at": "2026-01-02T14:15:00Z",
  "updated_at": "2026-01-02T14:15:00Z"
}
```

**PATCH /api/v1/suppliers/{id}**
```
Request (partial update):
{
  "credit_rating": "A",  /* Changed from A+ */
  "annual_revenue": 520000000  /* Changed */
}

Response:
{
  "id": "supplier_abc123",
  "credit_rating": "A",
  "annual_revenue": 520000000,
  "updated_at": "2026-01-02T15:00:00Z"
}
```

**GET /api/v1/suppliers?updated_since=2026-01-02T00:00:00Z**
```
Query suppliers that changed since timestamp (incremental sync).

Response:
{
  "suppliers": [
    { "id": "supplier_abc123", "name": "Acme Corp", "updated_at": "2026-01-02T14:15:00Z" },
    { "id": "supplier_xyz789", "name": "Beta Inc", "updated_at": "2026-01-02T10:30:00Z" }
  ],
  "pagination": {
    "limit": 100,
    "offset": 0,
    "total": 2
  }
}
```

### Versioning & Breaking Change Management

**Current: API v1 (2026-01)**

Supplier schema:
```json
{
  "id": "supplier_abc123",
  "external_id": "SAP-SUPPLIER-001",
  "name": "Acme Corp",
  "country": "US",
  "industry": "Manufacturing",
  "annual_revenue": 500000000,
  "credit_rating": "A+",
  "is_active": true
}
```

**Problem**: We want to rename `credit_rating` to `credit_score` and change the format from "A+" (string) to 90 (numeric score 0-100).

This is a breaking change. Old clients expect `credit_rating: "A+"`.

**Solution: Design v2**

**Backward Compatibility Strategy**:
1. In v1, add new field `credit_score_numeric` alongside old field `credit_rating`
2. Old clients continue using `credit_rating`; new clients use `credit_score_numeric`
3. After 12 months, v2 removes `credit_rating`; `credit_score` is the only field

**Step 1: v1 Enhanced (Backward Compatible)**

```json
{
  "id": "supplier_abc123",
  "external_id": "SAP-SUPPLIER-001",
  "name": "Acme Corp",
  "country": "US",
  "industry": "Manufacturing",
  "annual_revenue": 500000000,
  "credit_rating": "A+",  /* Old field, still present */
  "credit_score_numeric": 90,  /* New field, added */
  "is_active": true
}
```

**v1 requests continue to work**:
```
PATCH /api/v1/suppliers/supplier_abc123
{ "credit_rating": "A" }

Response:
{ "id": "supplier_abc123", "credit_rating": "A", "credit_score_numeric": 85, ... }

Old client works; ignores credit_score_numeric.
```

**New clients adopt v2**:
```
PATCH /api/v2/suppliers/supplier_abc123
{ "credit_score": 85 }  /* New format */

Response:
{ "id": "supplier_abc123", "credit_score": 85, ... }

v2 doesn't include deprecated credit_rating.
```

**Deprecation Timeline**:
```
2026-01: v1 Enhanced ships with both fields
2026-02: Announce: "credit_rating deprecated in v1; use credit_score in v2"
2026-09: "credit_rating will be removed in 6 months"
2027-01: v2 required; v1 deprecated
2027-02: v1 removed; v1 endpoints return 410 Gone
```

### Rate Limiting & Quotas

**ERP Integration Scenario**:

SAP ERP runs batch job daily, syncing 10k supplier changes:
```
Daily sync: 10,000 POST /api/v1/suppliers requests
Rate: ~100 requests/minute during sync window (1am-2am)

Standard rate limit: 1000 requests/minute
ERP integration exceeds limit; requests fail.

Problem: Manual intervention required; customer calls support.
```

**Solution: Bulk API with Higher Limits**

```
POST /api/v1/suppliers/bulk

Request (batch up to 100 suppliers):
{
  "suppliers": [
    { "external_id": "SAP-001", "name": "Supplier A", ... },
    { "external_id": "SAP-002", "name": "Supplier B", ... },
    ...
  ]
}

Response:
{
  "created": [
    { "external_id": "SAP-001", "id": "supplier_abc123" },
    ...
  ],
  "updated": [
    { "external_id": "SAP-003", "id": "supplier_xyz789" },
    ...
  ],
  "errors": [
    { "external_id": "SAP-999", "error": "Invalid country code" }
  ]
}

Rate limit: 10 bulk requests/minute (= 1000 suppliers/minute)
```

Now the ERP can sync 10k suppliers with only 100 bulk requests, within rate limits.

### Webhook Events

**Contract Risk Scored Event**

When AI risk scoring completes, emit an event so ERP can update supplier risk data in SAP:

```
Event: contract.risk_scored

Payload:
{
  "event_id": "evt_contracts_risk_001",
  "event_type": "contract.risk_scored",
  "timestamp": "2026-01-02T14:15:00Z",
  "data": {
    "contract_id": "contract_abc123",
    "supplier_id": "supplier_xyz789",
    "supplier_external_id": "SAP-SUPPLIER-001",  /* So ERP can match back to SAP */
    "risk_score": 75,
    "risk_category": "medium",
    "flagged_terms": ["liability_cap"],
    "changes": {
      "risk_score": { "before": null, "after": 75 },
      "flagged_terms": { "before": [], "after": ["liability_cap"] }
    }
  }
}

Delivery:
POST https://sap-ecc.acme.com/webhooks/zycus/contract-risk

Customer's SAP integration:
1. Receives webhook with supplier_external_id "SAP-SUPPLIER-001"
2. Looks up supplier in SAP by ID
3. Updates supplier risk score field in SAP custom extension
4. Creates workflow task: "Review Acme Corp contract; high risk flagged"
```

---

## Common Pitfalls

### Pitfall 1: API Without Versioning
**Anti-Pattern**: "Our API is v1.0 forever; we'll never break anything."

**Why it fails**: 18 months in, you need to add a required field to a request. All old clients break. You have to support both old and new behavior simultaneously, doubling complexity. After 3 months, you force a migration; customers churn.

**Right way**: Plan for versioning from day 1.
- Every endpoint is versioned: /api/v1/, /api/v2/
- Publish sunset dates: v1 supported until 2028-06-01
- When breaking, create v2 with overlap period (v1 and v2 both active for 6 months)
- Document migration guide for each breaking change

### Pitfall 2: Multi-Tenant Data Leakage
**Anti-Pattern**: 
```
GET /api/contracts?tenant=1234

Server code:
$contracts = Contract.where(tenant_id: params[:tenant]).all

If a customer submits tenant=OTHER_TENANT, they get OTHER_TENANT's data.
```

**Why it fails**: You've built a data exfiltration API. Competitor's employee creates account at your platform, submits different tenant IDs, steals contract data. You're sued. You lose the deal.

**Right way**: Tenant ID comes from auth token, not user input.
```
GET /api/contracts

Server code:
current_tenant = decode_auth_token(request.headers['Authorization']).tenant_id
$contracts = Contract.where(tenant_id: current_tenant).all

Even if customer submits ?tenant=OTHER_TENANT, it's ignored.
Auth token is the source of truth.
```

### Pitfall 3: Breaking Changes Without Migration Path
**Anti-Pattern**: 
```
April 1: Ship breaking change to API
"We removed the 'status' field. Use 'state' instead."

No deprecation warning. No migration period. Customers' integrations break immediately.
```

**Why it fails**: Customer finds out at 2am when their production data pipeline fails. Emergency call to engineering. Churn risk. Lost trust.

**Right way**: Deprecation path with clear timeline.
```
March 1: v1.5 ships with BOTH fields
- 'status' field is present (old code works)
- 'state' field is present (new code can adopt)
- Documentation: "status is deprecated; migrate to state by June 1"
- Monitoring: Track which customers still use 'status'

April 1: Email top 10 customers: "status deprecating in 8 weeks"

May 1: Auto-generated migration guides sent to all customers

June 1: v2 ships without 'status' field
- v1 still works; returns 'status'
- v2 is new default; has only 'state'
- Migration guide available for v1 to v2

September 1: v1 marked for sunset; "90 days left to migrate"

December 1: v1 endpoints return 410 Gone
```

### Pitfall 4: Rate Limiting That Breaks Integrations
**Anti-Pattern**: 
```
Standard rate limit: 100 requests/minute for all customers
ERP integration syncs 10k suppliers/day = 200+ requests/minute

Integration fails; customer is angry.
```

**Why it fails**: You've designed the API for individual users, not integrations. Enterprise customers integrate at scale; they need bulk operations.

**Right way**: Design tiered rate limiting.
```
Standard tier (self-service): 100 requests/min
Enterprise tier (contract): 10k requests/min + bulk APIs

Bulk API: POST /suppliers/bulk (up to 100 suppliers per request)
Rate: 1000 bulk requests/minute = 100k suppliers/minute

Now ERP can sync 10k suppliers with 100 bulk requests (within limit).
```

### Pitfall 5: No Idempotency for Retries
**Anti-Pattern**: 
```
Customer's integration retries a failed request.
No idempotency key; server creates the record again.
Customer ends up with duplicates.
```

**Why it fails**: Data integrity is broken. Customer has to manually clean up duplicates. They lose trust in the API.

**Right way**: Support idempotency keys.
```
POST /api/suppliers
Headers:
  Idempotency-Key: sap-batch-run-2026-01-02-14:15:00

Server:
- First call: Create supplier; store key → response in cache
- Retry call (same Idempotency-Key): Return cached response
Result: Customer can retry safely; duplicate prevention is automatic.
```

### Pitfall 6: Webhooks Without Signature Verification
**Anti-Pattern**: 
```
Customer's server receives webhook:
POST https://customer-server.com/webhooks

No signature verification. Any attacker can POST to this endpoint and trigger actions.
Attacker sends:
{ "contract_id": "abc123", "status": "deleted" }

Customer's system thinks contract was deleted; marks as invalid in SAP.
```

**Why it fails**: Webhooks become a security vulnerability. Customer's data integrity is compromised.

**Right way**: Sign all webhooks.
```
POST https://customer-server.com/webhooks

Headers:
  X-Signature: sha256=HMAC-SHA256(payload, customer_webhook_secret)
  X-Timestamp: 1234567890

Customer verifies:
1. Signature: HMAC-SHA256(payload, their_stored_secret) == X-Signature
2. Timestamp: Current time - X-Timestamp < 5 minutes (prevents replay attacks)

Only if both checks pass, process webhook.
```

### Pitfall 7: Ambiguous Error Codes
**Anti-Pattern**: 
```
POST /api/suppliers
Response 400 Bad Request:
{ "error": "Invalid input" }

Customer's integration doesn't know: Should I retry? Is it a permanent error?
Integration retries forever → backoff loop.
```

**Why it fails**: Customer's integration can't make intelligent retry decisions. Bad user experience.

**Right way**: Specific, parseable error codes.
```
Response 400 Bad Request:
{
  "error": {
    "code": "INVALID_COUNTRY_CODE",
    "message": "Country code 'ZZ' is not valid",
    "field": "country",
    "valid_values": ["US", "CA", "MX", ...]
  }
}

Customer's code:
if error.code == "INVALID_COUNTRY_CODE":
  # User error; don't retry
  alert_user("Fix country code")
elif error.code == "DATABASE_ERROR":
  # Server error; retry with backoff
  retry_with_backoff()
```

### Pitfall 8: No Pagination for Large Result Sets
**Anti-Pattern**: 
```
GET /api/contracts returns all 100,000 contracts for a large customer
Response JSON is 500MB
Timeout; customer integration fails
```

**Why it fails**: API is not designed for scale. Enterprise customers have millions of records.

**Right way**: Always paginate.
```
GET /api/contracts?limit=100&offset=0

Response:
{
  "contracts": [...],  /* 100 items */
  "pagination": {
    "limit": 100,
    "offset": 0,
    "total": 100000,
    "has_next": true,
    "has_prev": false
  }
}

Customer iterates: offset += 100 until has_next = false
```

---

## References

- **REST API Best Practices**: Roy Fielding, "Architectural Styles and the Design of Network-based Software Architectures"
- **HTTP Semantics**: RFC 9110, HTTP Semantics specification
- **JSON API Spec**: Standardized JSON API format
- **GraphQL Best Practices**: Apollo GraphQL documentation
- **API Versioning Strategies**: Zalando RESTful API Guidelines
- **Webhook Security**: OWASP Webhook Security

---

## Next Steps

1. **Design your first API endpoint**: Pick a business resource (contracts, suppliers, etc.)
2. **Specify request/response schemas**: Use the templates above
3. **Define rate limits**: Determine tiers and quotas for your use case
4. **Plan versioning strategy**: Document sunset dates for each major version
5. **Build integration test suite**: Test your API with customer scenarios (bulk sync, retries, rate limiting)
6. **Document breaking change policy**: Publish to all customers; honor it religiously
