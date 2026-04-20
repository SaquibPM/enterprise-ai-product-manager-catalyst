# AI Agent Design Document: Supplier Onboarding Verification Agent

**Version:** 1.0  
**Date:** 2026-04-12  
**Author:** Director of Product Management, Procurement AI Team  
**Status:** Ready for Engineering Review  
**Stakeholders:** CPO, Head of Finance, Chief Risk Officer, VP Engineering  

---

## Executive Summary

The Supplier Onboarding Verification Agent automatically validates new supplier applications in our enterprise procurement platform. It performs real-time verification of business registration, tax compliance, insurance certificates, and bank details against external data sources (government registries, D&B APIs, Stripe Connect). High-risk suppliers are routed to human reviewers; standard suppliers are approved within 90 seconds.

**Business Impact:** Reduces supplier onboarding cycle time from 5-7 business days to 2-4 hours; eliminates manual document review; enables 24/7 supplier signup.

**Key Design Decisions:**
1. Orchestrator-worker topology with 4 specialized verification workers (business reg, tax, insurance, banking)
2. 4-layer guardrails stack (input validation, output verification, cost control, compliance enforcement)
3. HITL on high-risk suppliers (>65% confidence threshold) and financial transfers >$10K
4. Progressive disclosure UX showing verification steps as they complete
5. Estimated cost: $0.32/supplier, scaling to $50K/month at 5K suppliers

**Key Risks & Mitigations:**
- *Risk:* False rejections harm supplier experience → *Mitigation:* Human appeal workflow + rapid re-verification
- *Risk:* Regulatory non-compliance (SOX, sanctions screening) → *Mitigation:* Compliance guardrails layer + monthly audit
- *Risk:* API cost runaway → *Mitigation:* Hard cost limit guardrails, request deduplication cache

---

## 1. Agent Architecture

### 1.1 System Topology

The agent uses an **Orchestrator-Worker architecture** with centralized coordination and 4 specialized parallel workers:

```
┌─────────────────────────────────────────────────────────────┐
│  SUPPLIER ONBOARDING ORCHESTRATOR                           │
│  (Routes requests, makes escalation decisions)              │
└────┬────────────────────────────────┬──────────────────┬────┘
     │                                │                  │
     ▼                                ▼                  ▼
┌──────────────────┐  ┌──────────────────┐  ┌───────────────┐
│ BUSINESS REG     │  │ TAX COMPLIANCE   │  │ INSURANCE     │
│ WORKER           │  │ WORKER           │  │ WORKER        │
├──────────────────┤  ├──────────────────┤  ├───────────────┤
│ • Verify Inc.,   │  │ • Check EIN/VAT  │  │ • Request GL  │
│   LLC, etc.      │  │ • Sanctions list │  │ • Validate    │
│ • Check SEC      │  │ • Penalties/liens│  │   coverage    │
│ • Verify address │  │ • Tax status     │  │ • Verify      │
│                  │  │                  │  │   expiry      │
└──────────────────┘  └──────────────────┘  └───────────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │ BANKING WORKER   │
                    ├──────────────────┤
                    │ • Verify bank acc│
                    │ • Check Stripe   │
                    │ • Microdeposit   │
                    │ • AML screening  │
                    └──────────────────┘
```

**Orchestrator Responsibilities:**
- Parse supplier application (company name, EIN, address, bank account)
- Fan-out requests to all 4 workers in parallel
- Aggregate results and compute risk score
- Decide: auto-approve, escalate to human, or reject

**Worker Pattern:**
Each worker is stateless and idempotent. Workers call external APIs (D&B, government registries, Stripe) with 30-second timeouts. If API fails, worker surfaces the failure (not a rejection) so orchestrator can escalate.

### 1.2 Autonomous Decision Points

| Decision | Trigger | Autonomy | Guardrails |
|----------|---------|----------|-----------|
| **Auto-approve supplier** | All workers return passing + confidence >85% + daily approvals <500 | Full (no HITL) | Cost limit, sanction check, daily volume cap |
| **Request re-verification** | Timeout from worker → retry with 60s timeout | Full | Max 2 retries per worker |
| **Escalate to human** | Confidence 50-85% OR financial transfer >$10K OR high-risk industry | Full | Flag reason, assign to risk team |
| **Auto-reject supplier** | Sanctions match OR business reg fraud signal OR AML fail | Full (with audit log) | Compliance guardrail layer only |
| **Pause verification** | Cost threshold exceeded in current hour | Full | Guardrail-triggered, requires manual cost reset |

### 1.3 Tool & API Inventory

| Tool | Provider | Purpose | Rate Limit | Avg Latency | Cost |
|------|----------|---------|-----------|-------------|------|
| Business Registry Lookup | Government APIs (50 states) | Verify incorporation, principals | 100 req/min | 800ms | $0.02/req |
| Tax ID Verification | IRS e-services + D&B | Verify EIN, tax status | 50 req/min | 1.2s | $0.05/req |
| Sanctions Screening | OFAC + EU sanctions lists | Match against SDN, EU lists | 200 req/min | 300ms | $0.01/req |
| Insurance Cert Retrieval | Insurer portals (50+ carriers) | Fetch GL certificates | 10 req/min | 2.5s | $0.08/req |
| Bank Account Verification | Stripe Connect API | Verify routing #, account | 100 req/min | 600ms | $0.03/req |
| AML Screening | AML Risk API | Check beneficial owners | 50 req/min | 900ms | $0.04/req |

**Total cost per supplier:** $0.23 in API calls + $0.09 overhead = **$0.32 average**

### 1.4 Memory Architecture

**Session Memory (short-lived, per request):**
- Supplier application input (name, EIN, address, bank account, industry)
- Worker response times and partial results (used for orchestrator timeout decisions)
- Decision trace: which workers passed, which failed, confidence calculation
- Cleared after HITL decision or auto-approval

**Conversational Memory (per supplier, 24-hour retention):**
- Appeal workflow state (if supplier challenges a rejection)
- Re-verification attempt count and results
- Human reviewer notes and reasons for escalation

**Long-term Memory (audit trail, indefinite):**
- All verification results stored in compliance audit database
- Used for: post-audit examination, fraud pattern detection, regulator inquiries
- Immutable, stored in encrypted cloud database

### 1.5 State Management

The agent maintains explicit state at each decision point:

```
States: PENDING_VERIFICATION → PROCESSING → APPROVED | ESCALATED | REJECTED
```

- **PENDING_VERIFICATION:** Application received, workers not yet started
- **PROCESSING:** Workers running in parallel; orchestrator waiting for quorum (3/4 minimum)
- **APPROVED:** All checks passed, supplier can transact immediately
- **ESCALATED:** Flagged for human review; supplier can transact after approval
- **REJECTED:** Failed compliance; supplier cannot transact; appeal workflow available

State transitions are atomic and logged to immutable audit trail.

---

## 2. Memory Architecture Detail

### 2.1 Session Memory (In-Process)

Stored in Redis with 1-hour TTL. Contains transient data needed for a single verification:

```json
{
  "supplier_id": "SUP-2024-001234",
  "application": {
    "legal_name": "Acme Manufacturing LLC",
    "ein": "12-3456789",
    "address": "100 Industrial Blvd, Chicago, IL 60601",
    "bank_account": "****1234"
  },
  "worker_results": {
    "business_reg_worker": {
      "status": "pass",
      "confidence": 0.98,
      "result": { "incorporation_date": "2015-03-22", "principals": [...] },
      "latency_ms": 850
    },
    "tax_worker": {
      "status": "pass",
      "confidence": 0.94,
      "result": { "tax_status": "good_standing", "no_liens": true },
      "latency_ms": 1200
    },
    ...
  },
  "orchestrator_decision": {
    "overall_confidence": 0.87,
    "recommendation": "escalate",
    "reasons": ["financial_transfer_threshold"]
  }
}
```

### 2.2 Conversational Memory (PostgreSQL)

Persistent record of supplier interactions, 24-hour availability for appeals:

- **Supplier ID** → Application submission time, results summary
- **Appeal workflow** → Reason for original rejection, appeal message, re-verification date, human decision
- **Re-verification state** → Which workers re-ran, why, results
- Indexed by supplier ID for fast lookup when supplier logs back in

### 2.3 Audit Trail (Immutable, Encrypted Cloud Storage)

Every verification event logged to S3 + CloudTrail:

```
timestamp | supplier_id | event | worker | confidence | decision | cost | reviewer_id
2026-04-12T14:23:15Z | SUP-2024-001234 | VERIFICATION_START | ORCHESTRATOR | - | - | - | -
2026-04-12T14:23:16Z | SUP-2024-001234 | WORKER_START | BUSINESS_REG | - | - | $0.02 | -
2026-04-12T14:23:17Z | SUP-2024-001234 | WORKER_PASS | BUSINESS_REG | 0.98 | PASS | - | -
...
2026-04-12T14:23:42Z | SUP-2024-001234 | ORCHESTRATOR_ESCALATE | - | 0.87 | ESCALATE | $0.32 | -
2026-04-12T14:25:00Z | SUP-2024-001234 | HUMAN_REVIEW | - | - | APPROVED | - | reviewer-jsmith@company.com
```

---

## 3. Tool & MCP Inventory

### 3.1 Tools by Category

**Government/Registry APIs:**
- Secretary of State APIs (50 states): Business registration verification
- IRS e-services: Tax ID + status lookups
- OFAC + EU Sanctions: Real-time sanctions matching

**Third-Party Data APIs:**
- Dun & Bradstreet Connect: Business profile + risk signals
- Stripe Connect API: Bank account verification + AML
- AML Risk API: Beneficial owner screening

**Internal Tools:**
- Supplier database (update post-approval)
- Document management (store GL certificates)
- Risk scoring engine (compute overall confidence)
- HITL workflow manager (route to reviewers)

### 3.2 Integration Points

| Integration | Direction | Frequency | SLA |
|-------------|-----------|-----------|-----|
| Supplier onboarding form | Inbound | Real-time | <100ms |
| Risk scoring engine | Outbound | Per verification | <500ms |
| HITL workflow queue | Outbound | Per escalation | <5s |
| Audit database | Outbound | Post-verification | Eventually consistent |
| Supplier DB update | Outbound | On approval | <30s |
| Finance system (AR setup) | Outbound | On approval | <60s |

---

## 4. Guardrails Design

### 4.1 Layer 1: Input Validation & Normalization

**Purpose:** Reject malformed, incomplete, or obviously invalid inputs before processing.

**Controls:**
- Field presence validation: All required fields (legal_name, EIN, address, bank) must be present
- Format validation: EIN must match XX-XXXXXXX pattern; routing number 9 digits; address contains street + city + state + ZIP
- XSS/injection defense: Strip special characters from text fields; validate bank account is numeric only
- Duplicate detection: Hash supplier name + EIN; reject if exact match exists in last 30 days
- Normalized inputs: Uppercase legal names, standardize address format (USPS), mask sensitive data before logging

**Failure mode:** Invalid inputs rejected with user-friendly error message (e.g., "EIN must be in format XX-XXXXXXX").

### 4.2 Layer 2: Prompt Injection & Adversarial Attack Defense

**Purpose:** Prevent attackers from manipulating agent behavior via poisoned supplier data.

**Controls:**
- No free-text fields flow to LLM prompts (all data is API-driven, not user-supplied text)
- Worker responses validated against schema before use (ensures D&B returns expected JSON structure)
- API response integrity checks: Validate digital signatures on high-value results (OFAC, tax)
- Jailbreak detection: Monitor for unusual token patterns in worker outputs (not applicable; workers return structured data only)
- Confidence floor: Even if all workers pass, confidence never exceeds input validation score (conservative scoring)

**Failure mode:** Suspicious API responses trigger escalation to human + alert to security team.

### 4.3 Layer 3: Output Filtering & Safety Checks

**Purpose:** Ensure agent decisions are correct before execution and don't violate business rules.

**Controls:**
- Confidence threshold enforcement: Auto-approve only if confidence >85%; 50-85% requires HITL
- Sanction check: No approval if any matching entity in OFAC/EU lists (hard gate)
- Business rule enforcement: No approval if supplier in high-risk industry (weapons, gambling) without explicit CPO sign-off
- AML match: If beneficial owner AML score >70, escalate regardless of other signals
- Output audit: Before executing approval, log orchestrator decision + all worker results + confidence calc

**Failure mode:** Suspicious decision patterns trigger alert to Chief Risk Officer (e.g., 10 approvals in 5 minutes from same IP).

### 4.4 Layer 4: Cost & Resource Control

**Purpose:** Prevent runaway API costs and resource exhaustion.

**Controls:**
- Per-request cost budget: Hard cap at $1.00 per supplier verification (current avg $0.32)
- Hourly cost limit: Max $500/hour (cap at 1,562 concurrent suppliers)
- Daily cost limit: Max $10K/day (prevents weekend/holiday surprise bills)
- Request deduplication cache: If same EIN verified in last 6 hours, return cached result (save $0.23)
- Timeout protection: Workers have 30-second timeout; if slower, return timeout (not error) and escalate
- Rate limit compliance: Respect all API provider rate limits; queue requests if approaching limit

**Failure mode:** Cost threshold exceeded → agent pauses all new verifications; alerts finance team; requires manual cost reset.

### 4.5 Layer 5: Compliance Enforcement

**Purpose:** Ensure all decisions comply with SOX, sanctions screening, AML, and data protection regulations.

**Controls:**
- Sanctions requirement: OFAC/EU sanction lists checked for every supplier (non-negotiable)
- AML requirement: Beneficial owner screening for suppliers >$50K annual spend
- SOX compliance: All approvals logged with timestamp, requester, decision reason, immutable audit trail
- Data retention: Audit logs retained for 7 years (SOX requirement)
- Data minimization: PII (SSN, passport) never stored; bank account digits masked after verification
- Export control: No verification for suppliers in sanctioned countries (hardcoded country list)

**Failure mode:** Compliance breach detected → immediate STOP all processing; alert Chief Risk Officer + Legal; manual review required.

### 4.2 Compliance Requirement Mapping

| Regulation | Agent Behavior | Guardrail Layer |
|-----------|---|---|
| **SOX (Financial controls)** | All approvals logged + immutable audit trail | Layer 5 |
| **OFAC (Sanctions)** | Every supplier screened against SDN list; no exceptions | Layer 3 + Layer 5 |
| **AML (Anti-money laundering)** | Beneficial owner screening for high-value suppliers | Layer 3 |
| **Data protection (GDPR/CCPA)** | PII masked; data deletion on appeal rejection | Layer 1 + Layer 5 |
| **ADA (Accessibility)** | Appeal workflow + human review path for all rejections | Design Layer |

### 4.3 Monitoring & Alerting

**Real-time monitoring:**
- Approval rate drift: Alert if day's approval rate >90% or <30% (indicates config problem)
- Confidence score distribution: Alert if median confidence drops >5% day-over-day
- API error rate: Alert if any single API fails >10% of requests
- Cost anomalies: Alert if hourly cost >$750 (50% above baseline)
- Latency spike: Alert if p95 latency >5 seconds

**Audit monitoring (daily):**
- Manual audit of 5% of approvals (randomized sample)
- Compliance checklist: 100% of high-value (>$50K) approvals reviewed
- Fraud signal detection: Check for coordinated re-submissions from same IP/email

---

## 5. Human Oversight Design

### 5.1 HITL Workflow Definition

The agent routes decisions to human reviewers based on explicit rules. All escalations go to the Risk Review Team (3 FTEs, SLA 2 hours).

```
Orchestrator output
        │
        ├─ Confidence >85% + all guardrails pass
        │  └─→ AUTO-APPROVE (HITL not triggered)
        │
        ├─ Confidence 50-85% OR high-risk industry OR financial transfer >$10K
        │  └─→ ESCALATE TO HUMAN (Priority: High) → Risk Review Team
        │      └─→ (after review) APPROVE or REJECT
        │
        └─ Confidence <50% OR sanctions match OR compliance fail
           └─→ AUTO-REJECT (with appeal workflow available)
```

### 5.2 Decision Approval Matrix

| Scenario | Decision Path | Confidence Threshold | Approval Time | Notes |
|----------|---|---|---|---|
| Standard supplier, all checks pass | Auto-approve | >85% | <2 minutes | No human needed |
| Partial match (e.g., AML alert on co-principal) | Escalate | 50-85% | 2 hours | Human judgment on false positive vs. real risk |
| Large transaction (>$10K) | Escalate | Any | 2 hours | Financial control requirement |
| High-risk industry (weapons, gambling, crypto) | Escalate | Any | 2 hours | CPO explicitly wants visibility |
| Sanctions match | Auto-reject | N/A | 1 minute | No discretion; automatic compliance |
| Business registration fraud signal | Auto-reject | N/A | 1 minute | D&B fraud flag = immediate reject |
| Appeal after rejection | Manual review | N/A | 4 hours | SLA: respond within 4 business hours |

### 5.3 Confidence & Risk Thresholds

**Confidence Score Calculation:**

```
overall_confidence = (
  business_reg_confidence * 0.25 +
  tax_compliance_confidence * 0.25 +
  insurance_confidence * 0.25 +
  banking_confidence * 0.25
)
```

Each worker returns confidence 0.0-1.0 based on match quality:
- **1.0:** Exact match on all fields
- **0.85:** Match on name + EIN; minor address variance
- **0.70:** Fuzzy match on name; all IDs match
- **0.50:** Partial match; some IDs missing/mismatched
- **0.0:** No match or timeout

**Risk Score Calculation:**

```
risk_score = 1.0 - (confidence * guardrails_multiplier)
```

Where guardrails_multiplier is 1.0 if all guardrails pass, 0.5 if any flag raised.

**Escalation thresholds:**
- confidence <50%: Always escalate (too risky)
- confidence 50-85%: Escalate (require human judgment)
- confidence >85%: Auto-approve (unless other flags)
- sanctions match: Auto-reject immediately
- AML score >70%: Always escalate
- Financial transfer >$10K: Always escalate

### 5.4 Approval SLAs

| Priority | Escalation Reason | SLA | Handling |
|----------|---|---|---|
| **Urgent** | Financial transfer >$100K | 30 minutes | Routed to Senior Reviewer + escalation manager |
| **High** | Standard escalation (confidence 50-85%) | 2 hours | Distributed to Risk Review Team queue |
| **Normal** | High-risk industry flagged | 4 hours | Queued for next business day |
| **Low** | Appeal after rejection | 4 business hours | Appeals specialist handles |

**SLA tracking:**
- Dashboard shows escalation queue depth, aging (time since escalation)
- Alert if any escalation >SLA by 30 minutes
- Daily report: # approved, # rejected, # time-out (escalation not reviewed in time → automatic approval)

### 5.5 Approval Routing Logic

**Routing rules:**

1. **By priority:** Urgent escalations → Senior Reviewer (1 FTE)
2. **By domain:** High-risk industry flags → CPO's team (on-call)
3. **By volume:** Standard escalations → Risk Review Team queue (round-robin)
4. **By complexity:** Appeals → Appeals Specialist (1 FTE)

**Reviewer UI for HITL:**
```
┌─────────────────────────────────────────────┐
│ ESCALATION REVIEW: SUP-2024-001234          │
├─────────────────────────────────────────────┤
│ Company: Acme Manufacturing LLC             │
│ Confidence: 0.73 [Escalation: Partial match]│
│                                              │
│ WORKER RESULTS:                              │
│ ✓ Business Reg: Incorporated 2015 (0.98)    │
│ ✓ Tax Compliance: Good standing (0.94)      │
│ ⚠ Insurance: GL cert not in system (0.45)   │
│ ✓ Banking: Account verified (0.92)          │
│                                              │
│ GUARDRAILS CHECK:                            │
│ ✓ Not sanctioned                            │
│ ✓ No AML concerns                           │
│ ⚠ Industry: Manufacturing (standard risk)   │
│                                              │
│ RISK TEAM NOTES:                             │
│ Insurance provider (Hartford) confirms       │
│ GL cert mailed; expect arrival in 2 days.   │
│                                              │
│ [ Approve ]  [ Reject ]  [ More Info ]      │
└─────────────────────────────────────────────┘
```

### 5.6 Escalation Paths & Fallback Behaviors

**If human reviewer unavailable (off-hours, overloaded):**
- Escalations >4 hours old trigger automatic fallback:
  1. **15 min after SLA:** Notify on-call manager
  2. **30 min after SLA:** Auto-escalate to SVP Finance
  3. **60 min after SLA:** Auto-approve with flag for post-audit review

**Fallback prevents blocking supplier onboarding** but requires post-implementation audit.

---

## 6. UX Design

### 6.1 Confidence Indicators

**Supplier facing (during verification):**

The supplier sees real-time progress as the agent verifies:

```
SUPPLIER ONBOARDING IN PROGRESS

[████████──] 80% Complete

Checking business registration...      ✓ Complete
Verifying tax compliance...            ✓ Complete
Requesting insurance documents...      ⟳ In progress (20s)
Verifying bank account...              ◯ Pending

Estimated completion: <60 seconds
```

After verification completes:

```
VERIFICATION RESULTS

Overall Confidence: 87% ✓ APPROVED

You can start transacting immediately.

Breakdown:
• Business registration:    ✓ 98% confidence
• Tax compliance:           ✓ 94% confidence
• Insurance:                ✓ 92% confidence
• Bank account verification: ✓ 90% confidence
```

### 6.2 Progressive Disclosure Patterns

When confidence is lower (escalation scenario), show reasoning:

```
VERIFICATION IN REVIEW

Your application is being reviewed by our Risk Team.
Confidence: 73% (Review required)

What happened:
We couldn't automatically verify your GL insurance
certificate. Our team is checking with Hartford
directly (usually 2 hours).

Next steps:
A specialist will review your application by 4:00 PM
today. You'll receive an email with the decision.

Questions? Contact: [supplier-support-team]
```

### 6.3 Error State Designs

For rejections, show clear reason + appeal path:

```
APPLICATION REJECTED

We were unable to verify your application.
Reason: Business registration not found for EIN 12-3456789

What this means:
We checked the IRS database and your EIN doesn't match
a current business registration. This could mean:
• EIN is incorrect
• Business not yet registered
• Recent formation (can take 2 weeks to appear)

What you can do:
1. Double-check your EIN [IRS lookup tool]
2. Appeal this decision [Submit appeal form]
   (We'll re-verify within 24 hours)
3. Contact support [Chat with specialist]

Appeal deadline: 30 days from this decision
```

### 6.4 User Control Mechanisms

Suppliers have explicit control points:

```
APPLICATION SETTINGS

[ ] I agree to re-verification if documents change
    (Check to auto-update; leave unchecked to require
     manual re-submission)

[ ] Notify me of escalations via email
    (Uncheck to only see updates in dashboard)

[ ] Allow 3rd-party verification services
    (Some services offer faster GL cert verification)
    
Withdraw application [Button]
Appeal decision [Button]
Download verification report [Button]
```

### 6.5 Confidence Visualization

Different components use color + icons to signal confidence:

**Icon system:**
- ✓ Green checkmark: >85% confidence, auto-approved
- ⚠ Yellow warning: 50-85% confidence, under review
- ✗ Red X: <50% confidence or compliance fail, rejected
- ⟳ Blue spinner: In progress, waiting for worker result

**Color system:**
- Green (#10B981): Approved, high confidence
- Yellow (#F59E0B): Escalated, medium confidence
- Red (#EF4444): Rejected, low confidence or compliance fail

### 6.6 Accessibility & Internationalization

**Accessibility:**
- All confidence indicators have text labels (not icon-only)
- Form fields have screen reader labels
- Color contrast meets WCAG AA (4.5:1 minimum)
- Keyboard navigation: Tab through all interactive elements

**Internationalization:**
- Support 5 languages (English, Spanish, French, German, Mandarin)
- Date/currency formatting by locale
- Legal terms translated by legal review (not auto-translate)

---

## 7. Cost Model

### 7.1 Per-Operation Cost Breakdown

| Component | Quantity | Unit Cost | Total |
|-----------|----------|-----------|-------|
| Business Reg verification (50 states) | 1 | $0.02 | $0.02 |
| Tax ID verification (IRS + D&B) | 1 | $0.05 | $0.05 |
| Sanctions screening (OFAC) | 1 | $0.01 | $0.01 |
| Insurance cert retrieval | 1 | $0.08 | $0.08 |
| Bank account verification (Stripe) | 1 | $0.03 | $0.03 |
| AML screening (beneficial owners) | 0.6* | $0.04 | $0.02 |
| **Subtotal (API costs)** | | | **$0.21** |
| Cloud infrastructure (compute, storage) | 1 | $0.06 | $0.06 |
| Human HITL (escalation cost amortized)** | 0.15 | $0.05 | $0.01** |
| **TOTAL PER SUPPLIER** | | | **$0.28-0.32** |

*AML screening only runs for suppliers >$50K annual spend (60% of total)*
**HITL escalation cost: assumes 15% escalation rate × 8 minutes human time @ $60/hour loaded cost = $0.016 amortized per supplier*

### 7.2 Monthly Projections at Scale

| Volume | API Cost | Infrastructure | HITL Cost | Total Monthly |
|--------|----------|---|---|---|
| 500 suppliers/month | $105 | $100 | $15 | **$220** |
| 1,000 suppliers/month | $210 | $150 | $30 | **$390** |
| 5,000 suppliers/month | $1,050 | $400 | $150 | **$1,600** |
| 10,000 suppliers/month | $2,100 | $600 | $300 | **$3,000** |

**Current run rate:** ~2,000 suppliers/month → **$640/month**

### 7.3 Cost Optimization Opportunities

1. **Request deduplication cache (Est. savings: 20%):** Cache results for 6 hours; reduce duplicate verifications from same organization
2. **Batch API calls (Est. savings: 10%):** Batch tax ID lookups (IRS accepts 100-at-a-time); saves API overhead
3. **Preferred vendor negotiation (Est. savings: 15%):** Negotiate volume discounts with D&B, Stripe for >5K suppliers/month
4. **Insurance cert self-service (Est. savings: $0.08):** Supplier uploads GL directly (reduces $0.08 API retrieval cost); verify via coverage amount

**Potential savings:** 45% reduction → **$0.15 per supplier** at 5K/month scale

### 7.4 Budget Guardrails

**Hard limits enforced by guardrail Layer 4:**
- Per-request cap: $1.00 (current avg $0.28, allows 3.5x growth before hitting limit)
- Hourly cap: $500 (prevents runaway costs during business hours)
- Daily cap: $10,000 (prevents weekend surprises)

If daily limit hit at 8:00 AM:
1. Agent pauses all new verifications (returns "System temporarily unavailable" to suppliers)
2. Alert sent to Finance + VP Engineering
3. Requires manual approval to reset cap
4. Post-incident review to identify cause (API pricing change, volume spike, etc.)

---

## 8. Operational Excellence

### 8.1 Monitoring & Observability

**Key metrics dashboard (updated real-time):**

```
SUPPLIER ONBOARDING AGENT — OPERATIONS DASHBOARD

┌──────────────────────────────────────────────────┐
│ Throughput          Approval Rate      API Costs  │
│ 1,250/day           87.3% approved     $18.50/hr  │
│ (↑12% vs. last week) (↓1.2% vs. baseline) (↓8%)   │
└──────────────────────────────────────────────────┘

Avg Latency: 42 seconds (p95: 68s, p99: 95s)
✓ SLA: <2 minutes (100% compliance this week)

Worker Health:
┌─────────────────┬────────┬─────────┬──────────┐
│ Worker          │ Uptime │ Avg Lat │ Error %  │
├─────────────────┼────────┼─────────┼──────────┤
│ Business Reg    │ 99.8%  │ 850ms   │ 0.3%     │
│ Tax Compliance  │ 99.1%  │ 1200ms  │ 1.2%     │
│ Insurance       │ 97.2%  │ 2500ms  │ 3.1%     │
│ Banking         │ 99.5%  │ 600ms   │ 0.5%     │
└─────────────────┴────────┴─────────┴──────────┘

Escalation Queue: 12 pending (SLA: 2 hours)
Oldest escalation: 34 minutes old ✓

Alerts (past 24h): None
```

**Logging & tracing:**
- Every request has trace ID (logs all worker calls, decisions, timings)
- All approvals logged to immutable audit database
- Worker response times logged (identify slow APIs)
- Cost tracking per API (identify cost drivers)

### 8.2 Incident Response Playbook

**Scenario: Worker timeout spike (e.g., Insurance API down)**

```
Detection: p95 latency >5s, insurance worker timeout rate >10%
Alert sent to: On-call SRE, Risk team lead
Time to respond: <5 min

Incident commander actions:
1. Page insurance API vendor support
2. Check vendor status page
3. If vendor issue: Auto-route escalations to manual review
   (with "Insurance vendor temporarily slow" note)
4. If our issue: Check recent deployments; consider rollback
5. ETA for resolution: Negotiate SLA
6. Monitor: Insurance worker error rate every 30s
7. Close incident: When error rate <1% for 10 min

Post-mortem: Root cause analysis + fix within 48h
(e.g., add circuit breaker to insurance API client)
```

### 8.3 Performance Benchmarks & Targets

| Metric | Current | Target Q2 2026 | Means |
|--------|---------|---|---|
| Avg verification latency | 42s | 30s | Parallel worker optimization |
| p95 latency | 68s | 50s | Better timeouts + circuit breakers |
| Approval rate | 87.3% | 89% | Fewer false rejections via threshold tuning |
| HITL escalation SLA compliance | 98% | 99%+ | Staffing + prioritization |
| API uptime (avg across all providers) | 99.2% | 99.5% | Vendor redundancy for critical paths |
| Cost per supplier | $0.32 | $0.18 | Cache + optimization (45% reduction) |
| Total monthly cost (5K suppliers) | $1,600 | $900 | Via optimization |

### 8.4 Rollout Plan

**Phase 1: Pilot (Week 1-2, 100 suppliers)**
- Release agent to internal testing; 3 internal procurement managers use it
- Monitor all metrics; manual verification of 100% of approvals
- Success criteria: Zero compliance breaches, <3 minute avg latency, 0 high-priority bugs

**Phase 2: Limited beta (Week 3-4, 500 suppliers)**
- Release to 10 customer accounts (hand-picked, tech-savvy)
- Monitor all metrics; human review of 10% sample of approvals
- Success criteria: 99% uptime, <60s avg latency, approval rate >85%

**Phase 3: Gradual rollout (Week 5-8, 5K suppliers)**
- Release to all customer accounts (5 new customers/day, capacity allowing)
- Ramp from 0 → 2,000 suppliers/month gradually
- Monitor: Cost, latency, approval rate; adjust staffing if escalation queue grows

**Phase 4: Full production (Week 9+, unlimited)**
- Open to all suppliers globally
- Continuous optimization: threshold tuning, cost reduction, UX improvements

---

## 9. Success Metrics

### 9.1 Business Metrics

| Metric | Target | Why It Matters |
|--------|--------|---|
| **Supplier onboarding time** | 2-4 hours (current: 5-7 days) | Revenue impact: faster supplier enablement = faster procurement |
| **% of suppliers auto-approved** | >85% | Efficiency: reduces manual review burden |
| **Supplier satisfaction (NPS on onboarding)** | >70 (baseline: 45) | Experience: transparent process increases trust |
| **Onboarding cost reduction** | 60% (from $150 → $60 per supplier) | Operational efficiency |

### 9.2 Technical Metrics

| Metric | Target | SLA |
|--------|--------|-----|
| **Verification latency (avg)** | <30 seconds | <2 min compliance requirement |
| **Verification latency (p95)** | <60 seconds | <5 min hard limit |
| **Worker uptime (each)** | 99%+ | <9 hours downtime/month |
| **HITL SLA compliance** | 98%+ | All escalations reviewed within SLA |
| **Cost per verification** | <$0.32 | <$0.50 budget limit |

### 9.3 Behavioral Metrics

| Metric | Target | Why It Matters |
|--------|--------|---|
| **Approval rate consistency** | 85-89% (not >95% or <80%) | Extreme rates indicate potential issues (too lenient or too strict) |
| **Appeal rate** | <2% | If higher, threshold too strict; if lower, possible compliance risk |
| **False reject rate (post-audit)** | <1% | Identifies incorrectly rejected suppliers (UX problem) |
| **False approve rate (fraud detected post-approval)** | <0.1% | Safety metric; unacceptable fraud leakage |

### 9.4 Measurement Plan

**Real-time tracking:**
- Live dashboard with all metrics above; updated every 60 seconds
- Alerts if any metric drifts >10% from target
- Weekly report to VP Procurement + CFO

**Manual audits:**
- Weekly: Random sample of 5% of approvals reviewed by human
- Monthly: Deep-dive analysis of approval rate trends, appeal patterns, cost drivers
- Quarterly: Comprehensive compliance audit (SOX, sanctions, AML) of 100% of high-value approvals

**Feedback loops:**
- Supplier feedback form post-verification (NPS question)
- Monthly sync with Risk Review Team on escalation patterns, false flags
- Quarterly review with Finance on cost optimization progress

---

## 10. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| **False rejections harm supplier experience** | High | Medium | Human appeal workflow; rapid re-verification SLA 4h |
| **Regulatory non-compliance (SOX, AML)** | Medium | High | Compliance guardrails layer; monthly audit; Legal review |
| **API cost runaway** | Medium | Low | Hard cost limits via guardrails; cache + optimization |
| **Worker API outage (e.g., IRS down)** | Medium | Medium | Circuit breaker; fallback to manual review; vendor diversification plan |
| **Sanctions/fraud leakage (approve bad actor)** | Low | Very High | Sanctions list always checked; secondary audit of >$50K approvals |
| **Prompt injection via supplier data** | Low | High | No LLM prompts; structured API responses only; input validation |
| **Escalation queue backlog (SLA miss)** | Medium | Medium | Monitor queue depth; escalate to VP if SLA at risk; on-call coverage |

---

## 11. Appendices

### A. Glossary

- **HITL (Human-In-The-Loop):** Agent escalates decisions to humans instead of auto-executing
- **Confidence score:** Agent's estimated certainty that supplier is legitimate (0.0-1.0)
- **Orchestrator:** Central agent component that coordinates worker decisions
- **Worker:** Stateless component that calls single external API (e.g., tax verification)
- **Escalation:** Routing a decision to human reviewer instead of auto-approval
- **Guardrail:** Safety control that prevents unsafe/non-compliant decisions
- **Latency (p95):** 95th percentile; 95% of requests complete in this time or less

### B. Regulatory Reference Guide

- **SOX (Sarbanes-Oxley):** Requires all financial control decisions logged + auditable
- **OFAC (Office of Foreign Assets Control):** U.S. sanctions screening; mandatory for all financial transactions
- **AML (Anti-Money Laundering):** Beneficial owner screening; required by FinCEN
- **GDPR:** EU data protection; requires consent for data processing + deletion on request

### C. Compliance Checklist

- [ ] SOX: All approvals logged + immutable audit trail ✓
- [ ] OFAC: Sanctions screening on 100% of suppliers ✓
- [ ] AML: Beneficial owner screening for >$50K suppliers ✓
- [ ] GDPR: PII handling; deletion workflow ✓
- [ ] Data retention: 7 years for audit trails ✓
- [ ] Incident response: <24h notification for breaches ✓

### D. Sample System Prompts

None (agent uses no LLM prompts; all API-driven, structured responses)

---

**Document prepared by:** Director of Product Management  
**Last updated:** 2026-04-12  
**Next review:** 2026-05-12 (post-pilot completion)  
**Approval:** CPO, CFO, Chief Risk Officer
