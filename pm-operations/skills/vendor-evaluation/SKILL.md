---
name: Vendor and Technology Partner Evaluation
slug: vendor-evaluation
description: Evaluate build vs. buy decisions, score vendors systematically, navigate security/compliance requirements, and conduct effective pilots
version: 1.0
tags: [vendor-management, procurement, build-vs-buy, enterprise, due-diligence]
---

# Vendor and Technology Partner Evaluation

## Purpose

Enterprise product decisions increasingly involve third-party vendors: AI/ML platforms, data infrastructure, identity management, compliance tools. Yet most PMs evaluate vendors poorly:

- Choosing based on demo quality or founder relationships (not fit-for-purpose)
- Ignoring total cost of ownership (license + integration + support)
- No security/compliance review (find problems 6 months into production)
- Skipping pilot evaluation (commit to platform before validating it works)
- No off-ramp planning (switching costs are prohibitive)

This skill teaches enterprise vendor evaluation using:
1. **Build vs. Buy Framework** — When to build custom vs. buy a vendor solution
2. **Vendor Scoring Matrix** — Systematic evaluation across 8+ dimensions
3. **Security/Compliance Checklist** — GDPR, SOC2, data processing agreements
4. **Pilot Planning** — How to test vendors before committing
5. **Vendor Management** — Ongoing oversight, risk monitoring, renegotiation

The framework is battle-tested across enterprise SaaS, procurement tech, and AI/ML platform decisions where vendor selection directly impacts product viability.

## Key Concepts

### 1. Build vs. Buy Framework

**Build** if:
- Vendor market is immature (no good options exist)
- Solution is core to your competitive moat (must own the IP/implementation)
- Your use case is highly differentiated (vendors don't support it)
- You have ML/engineering expertise in-house
- Timeline allows (12+ weeks for custom development)
- Total cost of ownership favors build (cheaper over 5 years than vendor)

**Buy** if:
- Problem is solved well by existing vendors (why reinvent)
- You lack in-house expertise (ML/infrastructure knowledge)
- Timeline is critical (vendor is 8 weeks, building is 16 weeks)
- Total cost is favorable (vendor <$X per year, build would cost more)
- Vendor is financially stable (proven track record, Series B+ funding)
- You can live with the vendor's roadmap (not fully customized)

**Build + Buy (Hybrid)** if:
- Vendor solves 70-80% of your problem
- You build 20-30% differentiation on top
- Integration is straightforward (API-first vendor)
- Vendor roadmap aligns with your needs

**Decision Tree:**
```
Does a vendor solution exist that covers 80%+ of requirements?
├─ NO → BUILD (or wait for market maturity)
│
├─ YES → Is vendor financially stable (Series B+, proven revenue, references)?
│   ├─ NO → BUILD (de-risk dependence on unstable vendor)
│   │
│   └─ YES → Is vendor pricing <$X/year (budget constraint)?
│       ├─ NO → BUILD (cost not justified)
│       │
│       └─ YES → Does vendor roadmap align with your direction?
│           ├─ NO → BUILD (or wait for vendor pivot)
│           │
│           └─ YES → Can you live with vendor's security/compliance posture?
│               ├─ NO → BUILD (de-risk compliance exposure)
│               │
│               └─ YES → Do you have integration resources for 4-week pilot?
│                   ├─ NO → BUILD (implement custom instead)
│                   │
│                   └─ YES → BUY + Vendor License Agreement
```

**Financial Scoring: Build vs. Buy**

Use Net Present Value (NPV) over 5 years:

```
BUILD OPTION:
Year 1: Development ($500K) + Infrastructure ($100K) + Team (1 FTE @ $200K) = $800K
Year 2-5: Infrastructure ($100K/yr) + Maintenance (0.5 FTE @ $100K) = $600K/yr
5-Year Total: $800K + (4 × $600K) = $3,200K

BUY OPTION:
Year 1: Vendor license ($250K) + Integration ($150K) = $400K
Year 2-5: License ($250K/yr) + Support ($50K/yr) = $300K/yr
5-Year Total: $400K + (4 × $300K) = $1,600K

NPV Calculation (assuming 10% discount rate):
BUILD: $3,200K (no depreciation of engineering investment)
BUY: $1,600K total cost
DECISION: BUY saves $1,600K over 5 years → Choose BUY unless strategic reasons favor BUILD

Note: Add additional costs (integration, training, switching costs) to BUY.
Adjust engineering costs based on your salary levels.
```

### 2. Vendor Evaluation Scorecard

Create a weighted scoring matrix. Each vendor gets evaluated on 8+ dimensions:

```
VENDOR EVALUATION SCORECARD

Vendor: [Vendor A] | [Vendor B] | [Vendor C]

═══════════════════════════════════════════════════════════════════════════════

1. FUNCTIONAL FIT (Weight: 30%)
   Does the vendor solve your core problem?

   Criteria:
   - Requirement A (extract clauses from contracts with 90%+ accuracy)
     Vendor A: Supports | Vendor B: Not supported | Vendor C: Requires customization
     Score: A=10, B=3, C=7
   
   - Requirement B (handles 50+ contract types)
     Vendor A: 12 types | Vendor B: 50+ types | Vendor C: 100+ types
     Score: A=6, B=10, C=10
   
   - Requirement C (API for integration)
     Vendor A: REST API | Vendor B: No API | Vendor C: REST + GraphQL
     Score: A=9, B=0, C=10
   
   - Requirement D (reporting/insights)
     Vendor A: Basic dashboard | Vendor B: Rich analytics | Vendor C: Limited
     Score: A=5, B=10, C=4
   
   FUNCTIONAL FIT SCORE (average):
   Vendor A: (10 + 6 + 9 + 5) / 4 = 7.5 / 10
   Vendor B: (3 + 10 + 0 + 10) / 4 = 5.75 / 10
   Vendor C: (7 + 10 + 10 + 4) / 4 = 7.75 / 10

   WEIGHTED SCORE (30% of total):
   Vendor A: 7.5 × 0.30 = 2.25 points
   Vendor B: 5.75 × 0.30 = 1.73 points
   Vendor C: 7.75 × 0.30 = 2.33 points

═══════════════════════════════════════════════════════════════════════════════

2. TECHNICAL FIT (Weight: 20%)
   Can your engineers integrate it? Does it meet performance requirements?

   Criteria:
   - API design / documentation quality (1-10)
     Vendor A: Well-documented, examples | Vendor B: Minimal docs | Vendor C: Excellent
     Score: A=8, B=4, C=9

   - Performance (latency, throughput)
     Requirement: <5sec response time, 1,000 req/min
     Vendor A: 3sec, 500 req/min (not sufficient) | Vendor B: 2sec, 2,000 req/min ✓
     Vendor C: 10sec (too slow) | Score: A=5, B=10, C=2

   - Scalability (can you grow without re-architecture)
     Vendor A: Scales to 10M events/day | Vendor B: Scales to 100M/day | Vendor C: Unknown
     Score: A=7, B=10, C=3

   - Technology stack alignment (languages, frameworks your team knows)
     Vendor A: Python/Kafka (our stack) | Vendor B: Go/gRPC (unfamiliar) | Vendor C: Node/REST (some overlap)
     Score: A=10, B=4, C=6

   TECHNICAL FIT SCORE (average):
   Vendor A: (8 + 5 + 7 + 10) / 4 = 7.5 / 10
   Vendor B: (4 + 10 + 10 + 4) / 4 = 7.0 / 10
   Vendor C: (9 + 2 + 3 + 6) / 4 = 5.0 / 10

   WEIGHTED SCORE (20% of total):
   Vendor A: 7.5 × 0.20 = 1.5 points
   Vendor B: 7.0 × 0.20 = 1.4 points
   Vendor C: 5.0 × 0.20 = 1.0 point

═══════════════════════════════════════════════════════════════════════════════

3. SECURITY & COMPLIANCE (Weight: 20%)
   Can you meet your security/compliance obligations?

   Criteria:
   - SOC2 Type II certified (1=No, 10=Yes, audited annually)
     Vendor A: Yes, audited 2024 | Vendor B: No | Vendor C: Yes, audited 2024
     Score: A=10, B=0, C=10

   - GDPR compliant (1=No, 10=Yes with DPA)
     Vendor A: DPA in place, EU data residency | Vendor B: No GDPR support | Vendor C: DPA exists, US-only
     Score: A=10, B=0, C=6

   - Data encryption (in transit and at rest)
     Vendor A: TLS + AES-256 encryption | Vendor B: TLS only | Vendor C: TLS + AES-256
     Score: A=10, B=5, C=10

   - Support for HIPAA / PCI / CCPA (if relevant to you)
     Vendor A: HIPAA ready | Vendor B: No | Vendor C: CCPA only
     Score: A=8, B=0, C=4

   - Security questionnaire responsiveness (how quickly do they answer)
     Vendor A: 48 hours | Vendor B: 2 weeks | Vendor C: 1 week
     Score: A=10, B=5, C=8

   SECURITY & COMPLIANCE SCORE (average):
   Vendor A: (10 + 10 + 10 + 8 + 10) / 5 = 9.6 / 10
   Vendor B: (0 + 0 + 5 + 0 + 5) / 5 = 2.0 / 10
   Vendor C: (10 + 6 + 10 + 4 + 8) / 5 = 7.6 / 10

   WEIGHTED SCORE (20% of total):
   Vendor A: 9.6 × 0.20 = 1.92 points
   Vendor B: 2.0 × 0.20 = 0.4 points
   Vendor C: 7.6 × 0.20 = 1.52 points

═══════════════════════════════════════════════════════════════════════════════

4. PRICING & COST (Weight: 15%)
   What's the total cost of ownership?

   Criteria:
   - License cost (Year 1)
     Vendor A: $250K/year | Vendor B: $500K/year | Vendor C: $200K/year
   
   - Integration/onboarding cost (one-time)
     Vendor A: $100K (4 weeks) | Vendor B: $200K (8 weeks) | Vendor C: $50K (2 weeks)
   
   - Ongoing support/maintenance
     Vendor A: 10% of license ($25K/year) | Vendor B: 20% ($100K/year) | Vendor C: 15% ($30K/year)
   
   - Professional services (training, customization)
     Vendor A: $0 (included) | Vendor B: $50K/year | Vendor C: $30K (optional)

   Total Cost of Ownership (Year 1):
   Vendor A: $250K + $100K + $25K + $0 = $375K
   Vendor B: $500K + $200K + $100K + $50K = $850K
   Vendor C: $200K + $50K + $30K + $30K = $310K

   PRICING SCORE (1-10, lower cost = higher score):
   Vendor A: $375K (middle cost) = 6/10
   Vendor B: $850K (highest cost) = 2/10
   Vendor C: $310K (lowest cost) = 8/10

   WEIGHTED SCORE (15% of total):
   Vendor A: 6 × 0.15 = 0.9 points
   Vendor B: 2 × 0.15 = 0.3 points
   Vendor C: 8 × 0.15 = 1.2 points

═══════════════════════════════════════════════════════════════════════════════

5. SUPPORT & ROADMAP (Weight: 10%)
   Can you rely on the vendor long-term?

   Criteria:
   - Support SLA (response time for critical issues)
     Vendor A: 2-hour response, 24/7 phone | Vendor B: 8-hour response, business hours | Vendor C: 4-hour response
     Score: A=10, B=4, C=7

   - Roadmap alignment (are they building what you need)
     Vendor A: Strong alignment (contract automation features on roadmap) | Vendor B: Weak alignment
     Vendor C: Medium alignment | Score: A=9, B=3, C=6

   - Customer success / account management
     Vendor A: Dedicated CSM + QBRs | Vendor B: Self-service | Vendor C: Email support
     Score: A=10, B=2, C=5

   - Community / ecosystem (3rd party integrations, plugins)
     Vendor A: Active community, 200+ integrations | Vendor B: Small community
     Vendor C: Medium community, 50 integrations | Score: A=9, B=3, C=6

   SUPPORT & ROADMAP SCORE (average):
   Vendor A: (10 + 9 + 10 + 9) / 4 = 9.5 / 10
   Vendor B: (4 + 3 + 2 + 3) / 4 = 3.0 / 10
   Vendor C: (7 + 6 + 5 + 6) / 4 = 6.0 / 10

   WEIGHTED SCORE (10% of total):
   Vendor A: 9.5 × 0.10 = 0.95 points
   Vendor B: 3.0 × 0.10 = 0.3 points
   Vendor C: 6.0 × 0.10 = 0.6 points

═══════════════════════════════════════════════════════════════════════════════

6. FINANCIAL VIABILITY (Weight: 5%)
   Will the vendor still exist in 3 years?

   Criteria:
   - Company stage / funding (Series A = high risk, Series C+ = lower risk)
     Vendor A: Series D, $150M funding, $20M ARR (stable) | Vendor B: Series B, $10M funding, $2M ARR (risky)
     Vendor C: Public company (IPO 2020), $500M revenue (very stable) | Score: A=8, B=3, C=10

   - Revenue / profitability (growing or contracting)
     Vendor A: 30% YoY growth, not yet profitable | Vendor B: 10% growth, cash burn
     Vendor C: 15% growth, profitable | Score: A=8, B=2, C=9

   - Customer concentration (single customer = risk)
     Vendor A: Top 3 customers = 25% revenue (healthy) | Vendor B: Top customer = 40% (risky)
     Vendor C: Diversified (no customer >5% revenue) | Score: A=8, B=2, C=10

   FINANCIAL VIABILITY SCORE (average):
   Vendor A: (8 + 8 + 8) / 3 = 8.0 / 10
   Vendor B: (3 + 2 + 2) / 3 = 2.3 / 10
   Vendor C: (10 + 9 + 10) / 3 = 9.7 / 10

   WEIGHTED SCORE (5% of total):
   Vendor A: 8.0 × 0.05 = 0.4 points
   Vendor B: 2.3 × 0.05 = 0.115 points
   Vendor C: 9.7 × 0.05 = 0.485 points

═══════════════════════════════════════════════════════════════════════════════

FINAL SCORECARD:

Dimension                    Weight    Vendor A    Vendor B    Vendor C
─────────────────────────────────────────────────────────────────────
Functional Fit               30%       2.25        1.73        2.33
Technical Fit                20%       1.5         1.4         1.0
Security & Compliance        20%       1.92        0.4         1.52
Pricing & Cost               15%       0.9         0.3         1.2
Support & Roadmap            10%       0.95        0.3         0.6
Financial Viability          5%        0.4         0.115       0.485
─────────────────────────────────────────────────────────────────────
TOTAL SCORE (out of 10):     100%      7.92/10     4.245/10    7.185/10

RANKING:
1. VENDOR A: 7.92/10 (Highest overall fit; best security/compliance)
2. VENDOR C: 7.185/10 (Good fit, lowest cost, but weaker technical implementation)
3. VENDOR B: 4.245/10 (Not recommended; security/compliance gaps, higher cost)

RECOMMENDATION: Choose VENDOR A
Rationale: 
- Highest security/compliance posture (SOC2 + GDPR + strong support)
- Best long-term support and roadmap alignment
- Only $65K more than Vendor C, but significantly better overall
- Functional and technical fit both strong
```

### 3. Risk Assessment for Selected Vendor

After selecting a vendor, document risks:

```
VENDOR A: RISK ASSESSMENT

1. Vendor Lock-in Risk (MEDIUM)
   - If we commit to Vendor A's API, switching costs would be $150K+ (integration rework)
   - Mitigation: Ensure API contracts include portability/data export rights; avoid vendor-specific SDKs
   
2. Cost Escalation Risk (MEDIUM)
   - Current price: $250K/year; typical vendor increases 10-20% annually
   - 5-year cost projection: $250K + 275K + 302K + 332K + 365K = $1,524K
   - Mitigation: Lock in 3-year price in initial contract; include cost cap clauses
   
3. Roadmap Misalignment Risk (LOW)
   - Vendor roadmap includes most of our requirements; low probability of misalignment
   - Mitigation: Quarterly business reviews to confirm roadmap alignment
   
4. Performance Degradation Risk (MEDIUM)
   - As we scale to 2,000 contracts/month, vendor API might slow down
   - Mitigation: Performance testing in pilot; SLA commitment in contract (include rate limits)
   
5. Data Security / Breach Risk (LOW)
   - SOC2 certified; strong encryption; regular audits
   - If breach occurs, would need to re-extract all contracts (high operational pain)
   - Mitigation: Require cyber insurance from vendor; include breach notification SLA
   
6. Vendor Dependency Risk (MEDIUM)
   - If vendor shuts down or gets acquired, we lose the service
   - Mitigation: Require data export / API access rights in contract; keep 3-month runway if vendor fails
```

### 4. Pilot Planning Template

**Objective:** Validate vendor solution works before long-term commitment (4 weeks, $50K budget)

```
PILOT PLAN: VENDOR A

GOAL: Validate that Vendor A extracts clauses with 90%+ accuracy on our contract types,
integrates with our platform, and costs <$0.50/document at scale.

TIMELINE: 4 weeks
- Week 1: Vendor setup, API integration, test data preparation
- Week 2: Live testing on 100 sample contracts
- Week 3: Accuracy measurement, performance benchmarking
- Week 4: Decision + full rollout planning

SUCCESS CRITERIA:
- Functional: Extraction accuracy ≥90% (tolerance: 85%+ acceptable with manual review)
- Performance: API response <5 seconds, supports 100 req/min
- Cost: ≤$0.50 per document
- Integration: <40 hours of engineering effort for initial setup
- Support: Vendor responds to technical issues within 4 hours

FAILURE SCENARIOS (triggers to stop pilot, go back to vendor selection):
- Accuracy <85% (deal breaker; feature doesn't work)
- Cost >$0.75/document (economics don't work)
- API response >10 seconds (too slow for user experience)
- Integration takes >80 hours (engineering time too high)
- Vendor doesn't respond to support requests (indicates poor service)

PILOT TEAM:
- PM (you): Requirements definition, vendor management, decision-making
- 1 Backend Engineer: API integration, testing setup
- 1 QA: Test data preparation, accuracy measurement
- Vendor: Technical support, API setup, troubleshooting

MEASUREMENTS:
1. Extraction Accuracy
   - Test on 100 diverse contracts (varied contract types, formats, lengths)
   - Manual review each extraction; classify as Correct / Partial / Incorrect
   - Calculate precision (% of extracted clauses that are correct)
   - Target: 90%+ precision

2. Performance
   - Latency: Record API response time for each contract (target: <5 sec)
   - Throughput: Test at 100 contracts/day (target: no errors, consistent latency)
   - Cost: Track API calls and calculate cost per document

3. Integration Effort
   - Time engineering spent: Initial setup, debugging, testing
   - Target: <40 hours total

4. User Experience
   - Pilot with 2-3 customer legal teams (informal feedback)
   - Ask: "Does this accelerate your work? Would you use this?"

DELIVERABLES:
- Pilot report (1 page: pass/fail against success criteria)
- Vendor scorecard (update with actual performance data)
- Integration documentation (API calls, auth, error handling)
- Cost model (actual cost per 100 contracts, projected annual cost)
- Recommendation memo (proceed with vendor, iterate, or reject)
```

## Application: Complete Vendor Evaluation Example

**SCENARIO:** You're evaluating 3 AI/ML platform vendors for adding contract clause extraction to your procurement platform. You have budget constraints, security requirements, and 8-week timeline.

### Step 1: Requirements Definition

```
VENDOR EVALUATION REQUIREMENTS
Contract Clause Extraction Platform

FUNCTIONAL REQUIREMENTS (Must-Have):
1. Extract payment terms, liability limits, renewal dates, termination clauses with 90%+ accuracy
2. Handle 50+ contract formats (PDF, Word, text)
3. REST API for integration
4. Dashboard for reviewing extraction results
5. Bulk processing (500+ contracts/month throughput)
6. Data export (all extracted clauses + metadata)

TECHNICAL REQUIREMENTS (Must-Have):
1. Latency <5 seconds per contract
2. Throughput: 1,000 requests/hour minimum
3. Uptime 99.5% SLA
4. API rate limiting (prevent abuse)
5. Webhook support (notify on completion)
6. Python SDK (our backend language)

COMPLIANCE REQUIREMENTS (Must-Have):
1. SOC2 Type II certified
2. GDPR compliant (EU data residency option)
3. Data Processing Agreement available
4. ISO 27001 certification
5. Annual security audit results available

NICE-TO-HAVE:
1. Custom model training
2. Multi-language support
3. Confidence scoring per extraction
4. Manual review queue / collaborative tools
5. Active learning (improve model based on corrections)

TIMELINE: Must ship solution to first customers by Week 8

BUDGET: <$500K Year 1 (license + integration + support)
```

### Step 2: Vendor Scorecard — Filled Example

```
VENDOR EVALUATION SCORECARD
Contract Clause Extraction Platform

═════════════════════════════════════════════════════════════════════════

VENDOR A: LegalTech AI
Series D, $20M ARR, Founded 2016, HQ: San Francisco

1. FUNCTIONAL FIT (Weight: 30%) — Score: 9.0/10
   
   Extract clauses with 90%+ accuracy → EXCEEDS (92% on benchmarks, 94% on our contract types)
   Handle 50+ formats → EXCEEDS (supports 100+ formats, including legacy scanned PDFs)
   REST API → YES (well-documented, SDKs in 5 languages)
   Dashboard → YES (strong dashboard, includes audit log)
   Bulk processing (500+/month) → EXCEEDS (supports 10,000+/month without issues)
   Data export → YES (JSON export, includes confidence scores and metadata)
   
   Unmet requirements: NONE (all requirements met or exceeded)
   
   SCORE: 9.0/10
   WEIGHTED: 9.0 × 0.30 = 2.70 points

2. TECHNICAL FIT (Weight: 20%) — Score: 8.5/10
   
   Latency <5 sec → YES (average 2.1 sec per contract, p99 4.3 sec)
   Throughput 1,000 req/hr → EXCEEDS (supports 5,000 req/hr on their platform)
   Uptime 99.5% → EXCEEDS (99.98% in past 12 months, SLA includes credits for downtime)
   API rate limiting → YES (documented limits, quota management)
   Webhook support → YES (supports webhooks, Kafka support available)
   Python SDK → YES (Python SDK + REST API)
   
   Issues discovered during evaluation:
   - Documentation could be more detailed (takes 2-3 hours to get first extraction working)
   - Error handling sometimes unclear (debugging latency issues took 1 hour with support)
   
   SCORE: 8.5/10 (strong technical fit, minor documentation gaps)
   WEIGHTED: 8.5 × 0.20 = 1.70 points

3. SECURITY & COMPLIANCE (Weight: 20%) — Score: 9.5/10
   
   SOC2 Type II → YES (audited annually, latest audit Sept 2024, clean bill of health)
   GDPR compliant → YES (DPA available, EU data residency option in Ireland)
   Data Processing Agreement → YES (standard DPA, can be customized for our needs)
   ISO 27001 → YES (certified, renewed 2024)
   Annual security audit → YES (available from request, no major findings)
   
   Additional security features discovered:
   - End-to-end encryption support (can encrypt data before sending to their API)
   - Regular penetration testing (annual + quarterly internal reviews)
   - Incident response SLA (notify customers within 2 hours of security incident)
   
   Compliance review: CISO reviewed their security posture; approved with conditions
   - Condition: Require Vendor A to include cyber liability insurance coverage (they agreed)
   
   SCORE: 9.5/10
   WEIGHTED: 9.5 × 0.20 = 1.90 points

4. PRICING & COST (Weight: 15%) — Score: 6.5/10
   
   License: $300K/year (enterprise plan, 2,000 API calls/month included)
   Integration: $150K (8-week engagement with their implementation partner)
   Ongoing support: 15% of license = $45K/year (included in package)
   Professional services: $50K (custom model training for your contract types)
   
   Year 1 Total Cost of Ownership: $300K + $150K + $45K + $50K = $545K
   
   Cost per document (at 500 contracts/month):
   $545K / (500 × 12) = $90.83 per contract (HIGH; target was <$50)
   
   Cost per document (if we scale to 2,000 contracts/month):
   $545K / (2,000 × 12) = $22.71 per contract (REASONABLE)
   
   5-year TCO (assuming 3% annual price increase):
   Y1: $545K | Y2: $560K | Y3: $577K | Y4: $594K | Y5: $612K
   5-year total: $2,888K
   
   Comparison:
   - Vendor B (RAG solution): $800K Year 1, but $0 scaling cost = $2,200K 5-year (20% cheaper)
   - Vendor C (Basic tool): $150K Year 1, but limited accuracy = not viable
   
   SCORE: 6.5/10 (expensive, but justified by accuracy + support; cheaper at scale)
   WEIGHTED: 6.5 × 0.15 = 0.975 points

5. SUPPORT & ROADMAP (Weight: 10%) — Score: 9.0/10
   
   Support SLA: 2-hour response for critical issues, 24/7 phone support (included with enterprise)
   Roadmap alignment: Strong alignment
   - Roadmap includes "liability limit extraction" (we need this Q2)
   - Roadmap includes "renewal date automation" (we need this Q3)
   - Roadmap includes "multi-language support" (nice-to-have)
   
   Customer success: Dedicated CSM + quarterly business reviews (included)
   
   Community: Active community, 300+ integrations available
   
   Reference customers:
   - Called [Customer X] (Acme Corp): "Great product, responsive support, have used for 1 year"
   - Called [Customer Y] (TechStartup): "Best in class, would not switch"
   
   SCORE: 9.0/10
   WEIGHTED: 9.0 × 0.10 = 0.90 points

6. FINANCIAL VIABILITY (Weight: 5%) — Score: 8.0/10
   
   Company stage: Series D, $150M in funding (strong financial position)
   Revenue: $20M ARR, growing 40% YoY (healthy growth)
   Profitability: Approaching break-even (not yet profitable, but strong unit economics)
   Customer concentration: Top 3 customers = 20% of revenue (healthy diversification)
   
   Risk assessment: Low probability of vendor failure (unlikely to shut down in next 3 years)
   
   SCORE: 8.0/10 (financially stable, though still pre-profitability)
   WEIGHTED: 8.0 × 0.05 = 0.40 points

═════════════════════════════════════════════════════════════════════════

VENDOR B: DataIntel (RAG-Based Solution)
Series B, $2M ARR, Founded 2021, HQ: Austin

1. FUNCTIONAL FIT — Score: 6.5/10
   Clause extraction accuracy: 78% (below our 90% requirement)
   Format support: 30 formats (below our 50+ requirement)
   API: Yes
   Dashboard: Basic
   Bulk processing: 1,000/month capability
   Data export: JSON export available
   
   Unmet requirements: Accuracy gap (need 90%, have 78%) is critical failure
   
   WEIGHTED: 6.5 × 0.30 = 1.95 points

2. TECHNICAL FIT — Score: 7.0/10
   Latency: 8 seconds average (EXCEEDS our 5-sec requirement, concerning)
   Throughput: 500 req/hr (below our 1,000 req/hr requirement)
   Uptime: No SLA provided (no commitment)
   API: REST API available
   Python SDK: No official SDK (community SDK exists)
   
   WEIGHTED: 7.0 × 0.20 = 1.40 points

3. SECURITY & COMPLIANCE — Score: 4.0/10
   SOC2: NO (not certified)
   GDPR: DPA available, but unclear compliance
   ISO 27001: NO (not certified)
   Security audit: No independent audit available
   
   CISO feedback: "Do not recommend for use with customer data. Insufficient security posture."
   
   WEIGHTED: 4.0 × 0.20 = 0.80 points

4. PRICING — Score: 8.5/10
   License: $100K/year (much cheaper)
   Integration: $50K
   Ongoing: $15K/year
   Y1 Total: $165K
   
   Cost per document (500/month): $27.50 (EXCELLENT)
   
   SCORE: 8.5/10 (cheapest option)
   WEIGHTED: 8.5 × 0.15 = 1.275 points

5. SUPPORT & ROADMAP — Score: 4.0/10
   Support: Email only, 24-hour response (no phone)
   SLA: No SLA provided
   Roadmap: Unclear what they're building next (no transparency)
   CSM: Self-service (no dedicated support)
   
   Reference customers: Small startups; can't get enterprise reference
   
   WEIGHTED: 4.0 × 0.10 = 0.40 points

6. FINANCIAL VIABILITY — Score: 3.0/10
   Series B company, high cash burn, runway ~18 months
   Revenue: $2M (small)
   High probability of failure if they can't raise Series C
   
   WEIGHTED: 3.0 × 0.05 = 0.15 points

═════════════════════════════════════════════════════════════════════════

VENDOR C: LegalAI Pro
Series A, $500K ARR, Founded 2023, HQ: NYC

1. FUNCTIONAL FIT — Score: 7.0/10
   Accuracy: 85% (below 90%, but acceptable with manual review)
   Formats: 25 formats (below 50+)
   API: Yes
   Dashboard: Yes
   Bulk processing: 300/month (below 500 requirement)
   
   WEIGHTED: 7.0 × 0.30 = 2.10 points

2. TECHNICAL FIT — Score: 5.5/10
   Latency: 6 seconds (slightly exceeds 5-sec requirement)
   Throughput: 200 req/hr (well below requirement)
   Uptime: No SLA
   API: REST, but limited documentation
   Python SDK: Community SDK only
   
   WEIGHTED: 5.5 × 0.20 = 1.10 points

3. SECURITY & COMPLIANCE — Score: 6.0/10
   SOC2: In progress (not yet certified, expected Q2 2025)
   GDPR: States they're GDPR compliant, but no DPA signed yet
   ISO 27001: No
   
   Risk: Compliance status is uncertain; CISO recommends deferring until certified
   
   WEIGHTED: 6.0 × 0.20 = 1.20 points

4. PRICING — Score: 9.0/10
   License: $50K/year (cheapest)
   Integration: $20K
   Y1 Total: $70K
   
   Cost per document: $11.67 (EXCELLENT)
   
   WEIGHTED: 9.0 × 0.15 = 1.35 points

5. SUPPORT & ROADMAP — Score: 5.5/10
   Support: Email + Slack (responsive, but no SLA)
   Founder-driven (if founder leaves, no support path)
   Roadmap: Committed to feature X (our need), timeline unclear
   
   WEIGHTED: 5.5 × 0.10 = 0.55 points

6. FINANCIAL VIABILITY — Score: 2.0/10
   Series A, runway ~24 months, high burn rate
   High probability of failure if Series B doesn't close
   Only 1 large customer (40% of revenue)
   
   WEIGHTED: 2.0 × 0.05 = 0.10 points

═════════════════════════════════════════════════════════════════════════

FINAL SCORES:
┌──────────────────────────────────────────┐
│ VENDOR A: 8.65 / 10 ✓ RECOMMENDED        │
│ VENDOR B: 5.95 / 10 (Not recommended)   │
│ VENDOR C: 6.40 / 10 (Not recommended)   │
└──────────────────────────────────────────┘

RECOMMENDATION: Choose VENDOR A

Rationale:
1. Highest overall score (8.65 vs. 6.40 vs. 5.95)
2. Meets all critical requirements (90%+ accuracy, security/compliance, performance)
3. Financially stable (unlikely vendor failure risk)
4. Best support and roadmap alignment
5. Premium pricing justified by superior product + support
6. CISO approved; Security review passed

Risks & Mitigations:
- Cost: $545K Y1 is high, but scales better (Y3 only $22.71/doc)
  Mitigation: Negotiate 3-year contract with price cap (max 5% annual increase)

- Vendor lock-in: Once we integrate, switching costs high
  Mitigation: Require data export rights; avoid proprietary SDKs; use REST API

- Performance at scale: May need to upgrade plan as volume grows
  Mitigation: Include scaling terms in contract (cost capping for volume growth)

Next Step: Proceed with 4-week pilot (VENDOR A), validate accuracy and cost assumptions
```

## Common Pitfalls and How to Avoid Them

### Pitfall 1: Choosing Based on Demo Quality
**What goes wrong:** Vendor gives great demo. Their salesperson is charismatic. You recommend them. Weeks later, you discover the demo was hand-crafted; real API is much slower.

**Why it happens:** Demos are designed to impress, not represent reality. You don't ask to see their code.

**How to fix it:** Always run a 4-week pilot on your own data before committing. Demos are great for screening, but pilots are essential.

**Right approach:**
- Demo screening (thumbs up/down) → Shortlist top 2-3 vendors → Run 4-week pilots → Make decision based on pilot results

### Pitfall 2: Ignoring Total Cost of Ownership
**What goes wrong:** Vendor A costs $200K/year. Vendor B costs $400K/year. You choose Vendor A to save money. But Vendor A requires $300K in integration work, while Vendor B is plug-and-play.

**Why it happens:** You focus on license cost (visible) and ignore integration/support costs (hidden).

**How to fix it:** Create TCO spreadsheet including: license + integration + ongoing support + professional services + switching costs (if you leave).

**Right approach:**
```
Vendor A: $200K/yr license + $300K integration = $500K Y1 + $200K/yr ongoing = $1.3M over 5 years
Vendor B: $400K/yr license + $50K integration = $450K Y1 + $400K/yr ongoing = $2.45M over 5 years
Vendor A is cheaper despite higher license cost.
```

### Pitfall 3: No Compliance Review
**What goes wrong:** You choose Vendor X. 6 months in, your CISO discovers they're not SOC2 certified. Now you're rearchitecting to move customer data off their platform.

**Why it happens:** You assume vendors handle compliance. They don't always. You didn't ask.

**How to fix it:** Add Compliance & Security section to EVERY vendor evaluation. Have CISO sign off before decision.

**Right approach:**
- SOC2 Type II certification (required)
- GDPR/CCPA compliance + DPA
- Data encryption (in transit + at rest)
- Annual security audit
- CISO sign-off before commitment

### Pitfall 4: Skipping the Pilot
**What goes wrong:** Vendor looks great on paper. You sign 3-year contract. In week 2, you discover their API is 50% slower than claimed. Now you're stuck for 2.75 more years.

**Why it happens:** You think pilots are expensive and slow. Committing seems faster.

**How to fix it:** Pilots are insurance. 4 weeks + $50K now saves you from bad decisions that cost $500K+ over time.

**Right approach:**
- Always run 4-week pilot on your actual use case
- Success criteria: accuracy targets, performance targets, cost targets
- Fail-fast (stop if criteria aren't met)
- Make decision based on pilot, not demo

### Pitfall 5: Vendor Lock-in Without Mitigation
**What goes wrong:** You build your product tightly integrated with Vendor X's APIs. 2 years later, they raise prices 100% or shut down. You're stuck.

**Why it happens:** Integration is convenient. You don't think about what happens if the vendor fails.

**How to fix it:** Design for portability. Use abstraction layers. Ensure data export rights in contract.

**Right approach:**
```
Design Pattern: API Abstraction Layer

Instead of:
  Feature.call_vendor_api() → Vendor X

Design:
  Feature.call_extraction_service()
    → ExtractionServiceAdapter (interface)
      → VendorXAdapter (implementation)
      → VendorYAdapter (alternative implementation)

This way, switching vendors only requires swapping the adapter.
```

### Pitfall 6: Vendor Evaluation Without Stakeholder Buy-in
**What goes wrong:** You evaluate vendors and recommend Vendor A. Engineering wanted Vendor B. Security wanted Vendor C. Now everyone disagrees.

**Why it happens:** You make the evaluation alone instead of involving stakeholders.

**How to fix it:** Evaluation is a group sport. Get alignment on scoring weights and success criteria BEFORE evaluation.

**Right approach:**
```
Week 1: Stakeholder workshop
- Engineering: What are technical must-haves?
- Security: What are compliance requirements?
- Finance: What's budget? What's ROI threshold?
- Product: What's strategic importance?

Week 2-3: Create shared evaluation scorecard
- All stakeholders agree on scoring criteria and weights

Week 4: Evaluate vendors
- All stakeholders present; discuss scoring as you go

Week 5: Decision
- Recommendation comes from shared analysis, not PM alone
```

## References and Further Reading

1. **The Enterprise Software Evaluation Playbook** (G2, 2023)
   https://www.g2.com
   Vendor evaluation best practices from 1000+ enterprise companies

2. **Build vs. Buy: A Practical Framework** (Sequoia Capital, 2022)
   https://www.sequoiacap.com
   Financial model for build vs. buy decisions

3. **Negotiating Vendor Contracts** (TechSoup, 2023)
   How to negotiate better terms with vendors

4. **SOC2 Compliance Checklist** (CloudSecurity Alliance)
   Security requirements for evaluating vendor compliance posture

5. **GDPR Data Processing Agreements** (EU GDPR Portal)
   Template and guidance for vendor data processing agreements

6. **The Procurement Playbook** (Buying Excellence, 2023)
   Enterprise software procurement best practices
