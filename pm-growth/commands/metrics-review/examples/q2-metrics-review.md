# Q2 2026 Metrics Review: Procurement Analytics Platform

**Period**: April - June 2026
**Prepared by**: VP Product + Analytics
**Date**: July 1, 2026
**Review Audience**: Executive Team, Product, Engineering

---

## Executive Summary

Q2 saw strong revenue growth (32% YoY) but slower customer acquisition than target. Contract scoring delays (pushed to Q3) created a 12% gap in new customer acquisition vs. target. Retention remained healthy at 96%. Key opportunities: (1) Accelerate AI contract scoring launch (unblocks 3 deals, $450M potential), (2) Improve sales conversion rate (currently 18%, target 22%).

---

## Metric Tree

```
COMPANY HEALTH
├── REVENUE METRICS
│   ├── Monthly Recurring Revenue (MRR)
│   │   ├── MRR Growth (QoQ, YoY) ✓
│   │   ├── ARPU (Average Revenue Per User) ✓
│   │   └── Revenue Expansion Rate (upsells) ✓
│   └── Annual Recurring Revenue (ARR)
│       ├── New ARR ✗ (below target)
│       ├── Churned ARR (churn)
│       └── Expanded ARR (expansion revenue)
│
├── CUSTOMER METRICS
│   ├── Customer Acquisition
│   │   ├── New Customers ✗ (12 below target)
│   │   ├── Sales Conversion Rate ✗ (18% vs 22% target)
│   │   └── Sales Cycle Length (64 days, on target)
│   ├── Customer Retention
│   │   ├── Retention Rate ✓ (96% healthy)
│   │   ├── Churn Rate ✓ (4%, acceptable)
│   │   └── NPS ✓ (62, healthy)
│   └── Customer Growth
│       ├── Expansion ARR ✓ (7% customer growth)
│       └── Land & Expand Rate ✓ (5/12 = 42% of new customers expand)
│
├── PRODUCT METRICS
│   ├── Feature Adoption
│   │   ├── AI Contract Scoring Readiness ✓ (design complete, coding starts May)
│   │   ├── Spend Analytics Adoption ✓ (78% of customers use, target 70%)
│   │   └── New Feature Usage (avg 3 weeks to first use) ✓
│   ├── Product Performance
│   │   ├── Platform Uptime ✓ (99.94%, target 99.9%)
│   │   ├── API Response Time ✓ (450ms p99, target < 500ms)
│   │   └── Customer-Reported Bugs (10 medium, 0 critical) ✓
│   └── Product Health
│       ├── Customer Support CSAT ✓ (4.2/5, target 4.0+)
│       ├── Support Response Time ✓ (3.2 hours, target < 4 hours)
│       └── Feature Bugs (new defects per sprint) ✓ (avg 1.2/sprint)
│
├── FINANCIAL METRICS
│   ├── Unit Economics
│   │   ├── CAC (Customer Acquisition Cost) ✓ ($15k, target < $20k)
│   │   ├── LTV (Lifetime Value) ✓ ($180k, 12:1 LTV:CAC ratio)
│   │   └── Payback Period ✓ (10 months, target < 12 months)
│   └── Burn & Runway
│       ├── Monthly Burn ✓ ($50k, on track for breakeven)
│       ├── Cash Runway ✓ (18 months given current burn)
│       └── Gross Margin ✓ (72%, healthy)

Legend: ✓ = On target | ✗ = Below target
```

---

## REVENUE METRICS

### MRR & ARR Performance

| Metric | Q2 2026 | Q1 2026 | Target | YoY | Status |
|--------|---------|---------|--------|-----|--------|
| MRR | $425k | $394k | $420k | +32% | On Target ✓ |
| ARR | $5.1M | $4.7M | $5.0M | +28% | Above Target ✓ |
| ARPU | $12.8k | $12.5k | $12.5k | +7% | On Target ✓ |
| Expansion ARR | $180k | $165k | $175k | +15% | On Target ✓ |

**Analysis**: 
- MRR growth 32% YoY is strong (healthy company is 10-15% MoM)
- ARPU grew 7% (customers upgrading to premium tiers) ✓
- Expansion revenue grew 15% (upsells from existing customers working)

### New Customer Revenue (Concerning)

| Metric | Q2 2026 | Q1 2026 | Target | Status |
|--------|---------|---------|--------|--------|
| New ARR | $520k | $480k | $580k | ✗ Below Target |
| New Customers | 12 | 14 | 15 | ✗ Below Target |
| ARR per New Customer | $43.3k | $34.3k | $38.7k | ✓ Above Target |

**Analysis**: 
- New customer count (12) is 20% below target (15)
- BUT: Average ARR per customer is 12% above target ($43.3k vs $38.7k)
- Root cause: Fewer customers, but larger deal size (positive signal)
- New ARR is 10% below target ($520k vs $580k), but driven by volume not quality

---

## CUSTOMER ACQUISITION METRICS

### Sales Funnel (Concerning)

| Stage | Q2 | Target | Conversion | Status |
|-------|-----|--------|------------|--------|
| Marketing Qualified Leads (MQLs) | 850 | 800 | - | ✓ Above Target |
| Sales Qualified Leads (SQLs) | 220 | 200 | 26% | ✓ Above Target |
| Sales Opportunities (Opps) | 65 | 60 | 30% | ✓ Above Target |
| Closed-Won Deals | 12 | 15 | 18% | ✗ Below Target |

**Analysis**:
- Top of funnel (MQLs, SQLs, Opps) is strong (all above target)
- **Issue is conversion at bottom of funnel: 18% close rate vs 22% target**
- **Loss reason**: AI contract scoring not yet available (3 deals, $450M potential stuck)
- If contract scoring launches Q3 as planned, expect 22%+ close rate recovery

**Root Cause**:
- 3 deals explicitly waiting for AI contract scoring (deal blockers)
- These are high-value deals ($150M+ each)
- Not a sales quality issue; product roadmap issue

**Recommendation**: 
- Ensure AI contract scoring ships on schedule (Aug 1 beta, Aug 31 GA)
- Expected impact: Unblock 3 deals ($450M ARR), recover to 22%+ close rate in Q3

### Sales Cycle & Deal Size

| Metric | Q2 | Q1 | Target | Status |
|--------|-----|-----|--------|--------|
| Sales Cycle | 64 days | 62 days | 60 days | ✓ On Target |
| Avg Deal Size (New) | $43.3k ARR | $34.3k ARR | $38.7k ARR | ✓ Above Target |
| Deal Size Range | $18k - $180k | $20k - $120k | $25k - $150k | ✓ Healthy |

**Analysis**:
- Sales cycle stable (64 days is normal for enterprise)
- Deal size trending up (larger customers, positive)
- Deal mix: 3 enterprise (>$100k), 5 mid-market ($20-100k), 4 lower mid-market (<$20k)

---

## CUSTOMER RETENTION METRICS

### Retention & Churn (Healthy)

| Metric | Q2 | Q1 | Target | Status |
|--------|-----|-----|--------|--------|
| Gross Retention | 96% | 96% | 95%+ | ✓ On Target |
| Net Retention (with expansion) | 104% | 102% | 100%+ | ✓ On Target |
| Churn Rate (MRR) | 4% | 4% | <5% | ✓ On Target |
| NPS (Net Promoter Score) | 62 | 58 | 60+ | ✓ On Target |

**Analysis**:
- 96% retention is healthy (enterprise SaaS benchmark: 90-95%)
- 104% net retention means expansion revenue > churn (great sign)
- Churn drivers: 1 healthcare customer (budget cut), 1 mid-market (switching to Coupa)
- NPS 62 is strong (SaaS benchmark: 40-50)

**Churn Breakdown**:
- Total churned customers: 2 of 32 (6.25% customer churn)
- Total churned MRR: $50k of $425k MRR (11.8% MRR churn)
- Healthcare customer churn: $30k (budget cut, not product quality)
- Mid-market churn: $20k (switching to Coupa, but won back 1 larger Coupa customer)

---

## PRODUCT ADOPTION METRICS

### Feature Adoption

| Feature | Adoption | Target | Status |
|---------|----------|--------|--------|
| Spend Analytics | 78% of customers | 70% | ✓ Above Target |
| Supplier Risk Scoring | 45% of customers | 45% | ✓ On Target |
| Compliance Reporting | 32% of customers | 35% | ✗ Slightly Below |
| Custom Dashboards | 55% of customers | 50% | ✓ Above Target |
| Forecasting (New in Q2) | 12% of customers (first month) | 15% | ✓ On Target |

**Analysis**:
- Spend Analytics is most-used feature (78% adoption)
- Forecasting launched late in Q2 (April 15), early adoption at 12% is strong
- Compliance Reporting at 32% (vs 35% target) is minor miss, likely due to customer education gap

**Recommendations**:
1. Create compliance reporting tutorial (2-minute video) to boost adoption
2. Target compliance reporting feature in healthcare/government verticals (higher compliance need)
3. Expected impact: Boost compliance reporting to 40% adoption by Q3

---

## PRODUCT PERFORMANCE METRICS

### System Health

| Metric | Q2 | Target | Status |
|--------|-----|--------|--------|
| Platform Uptime | 99.94% | 99.9% | ✓ On Target |
| API Response Time (p99) | 450ms | <500ms | ✓ On Target |
| Mean Time to Resolution (MTTR) | 90 min | <120 min | ✓ On Target |
| Critical Bugs (active) | 0 | 0 | ✓ On Target |

**Analysis**:
- System reliability is strong (99.94% uptime)
- Performance is optimal (450ms response time)
- 4 hours total downtime this month (2 incidents: database replication, cache timeout)
- Both incidents resolved quickly (<2 hours each)

**Incidents This Quarter**:
1. Database replication lag during peak load (May 15) — Infrastructure team added more replicas
2. Cache layer timeout under high concurrency (June 10) — Engineering added cache layer capacity

---

## FINANCIAL METRICS

### Unit Economics (Healthy)

| Metric | Q2 | Target | Status |
|--------|-----|--------|--------|
| CAC (Customer Acquisition Cost) | $15k | <$20k | ✓ On Target |
| LTV (Lifetime Value) | $180k | >$150k | ✓ On Target |
| LTV:CAC Ratio | 12:1 | >10:1 | ✓ On Target |
| Payback Period | 10 months | <12 months | ✓ On Target |
| Gross Margin | 72% | 70%+ | ✓ On Target |

**Analysis**:
- LTV:CAC ratio of 12:1 is excellent (healthy SaaS: 3:1+)
- Payback period 10 months is healthy
- Gross margin 72% is healthy for SaaS
- CAC declining (Q1: $18.5k, Q2: $15k) due to better sales efficiency

### Burn & Runway

| Metric | Q2 | Target | Status |
|--------|-----|--------|--------|
| Monthly Burn | $50k | <$100k | ✓ Healthy |
| Runway | 18 months | >12 months | ✓ On Target |
| Breakeven | 6-9 months | <12 months | ✓ On Track |

**Analysis**:
- Company is on path to breakeven within 9 months
- Current cash runway: $900k ÷ $50k/month = 18 months
- At current MRR growth (32% YoY), breakeven by October 2026

---

## ROOT CAUSE ANALYSIS: New Customer Acquisition Miss

### The Issue
- Target: 15 new customers per quarter
- Actual: 12 new customers (80% of target, 12% miss)
- Root cause: 3 deals stuck waiting for AI contract scoring feature

### The Data
**Blocked Deals**:
1. Financial Services company ($850M ARR) - Waiting for AI contract scoring for SOX compliance
2. Healthcare company ($450M ARR) - Waiting for AI + healthcare compliance rules
3. Government Contractor ($2.1B ARR) - Waiting for AI + government compliance rules

**Positive Signal**: 
- These are our largest deals (avg $150M+ potential each)
- Deals are "in negotiation" status, not lost
- Sales team confident they'll close once AI contract scoring ships

### Why This Happened
- AI contract scoring was originally planned for Q2
- Delayed to Q3 due to: (1) ML model training data collection took longer, (2) Engineering resources reallocated to platform reliability work
- Decision was correct trade-off: Platform uptime SLA is table-stakes for enterprise

### Expected Recovery
- AI contract scoring: Beta Aug 1, GA Aug 31
- Healthcare compliance rules: GA Sept 15
- Government compliance rules: GA Oct 1

**Expected impact**: All 3 deals close by Sept/Oct, adding $450M potential ARR to pipeline

---

## RECOMMENDATIONS & ACTIONS

### Priority 1: Ship AI Contract Scoring On Schedule (Aug 1/31)
**Why**: Unblocks $450M+ potential ARR in 3 deals
**Action**: Weekly sync with engineering, track sprint velocity
**Owner**: VP Product
**Timeline**: Weekly check-ins starting May 1
**Expected Impact**: +3 customers, +$130k ARR by Q3

### Priority 2: Improve Sales Conversion Rate (18% → 22%)
**Why**: Recover to target conversion rate once contract scoring ships
**Action**: (1) Sales training on AI positioning (Aug 1-15), (2) Competitive messaging docs (Aug 15)
**Owner**: VP Sales
**Timeline**: Training complete by Aug 22
**Expected Impact**: +2-3 customers per quarter, +$80-120k ARR

### Priority 3: Boost Compliance Reporting Adoption (32% → 40%)
**Why**: Minor gap, easy win (just needs education)
**Action**: (1) Create 2-min compliance reporting tutorial, (2) Email campaign to healthcare/government customers
**Owner**: Product + Customer Success
**Timeline**: Tutorial by July 15, campaign by Aug 1
**Expected Impact**: +1-2 customer expansion, +$20k ARR

### Priority 4: Continue Retention Excellence
**Why**: 96% retention is strong; maintain momentum
**Action**: (1) Quarterly NPS surveys, (2) Healthcare customer success check-ins (at-risk segment)
**Owner**: Customer Success
**Timeline**: Ongoing
**Expected Impact**: Maintain 96%+ retention, prevent future churn

---

## Looking Ahead: Q3 Targets

**Based on Q2 performance and Q3 initiatives**:

| Metric | Q3 Target | Rationale |
|--------|-----------|-----------|
| MRR | $550k | 15% QoQ growth (plus AI contract scoring impact) |
| ARR | $6.6M | $1.5M from new customers (contract scoring unblock) |
| New Customers | 18 | +3 from contract scoring, +3 from conversion rate improvement |
| Conversion Rate | 22% | Recovery with AI positioning |
| Retention | 96%+ | Maintain current performance |
| NPS | 65+ | Improvement from contract scoring feature |

**Q3 Initiatives Driving Growth**:
1. AI contract scoring launch (Aug 1 beta, Aug 31 GA)
2. SAP Ariba integration (Aug 15) — unblocks 1 deal
3. Healthcare compliance rules (Sept 15) — unblocks 1 deal
4. Sales team training & messaging (Aug 1-22)

---

## Metric Health Summary

| Category | Status | Trend | Action Required |
|----------|--------|-------|-----------------|
| Revenue | On Target ✓ | Growing (+32% YoY) | None, maintain momentum |
| Customer Acquisition | Below Target ✗ | Temporary (roadmap blocker) | Ship AI contract scoring Q3 |
| Customer Retention | Healthy ✓ | Stable (96%) | Monitor healthcare segment |
| Product Performance | Excellent ✓ | Improving | Continue reliability work |
| Financial Health | Strong ✓ | Improving (path to breakeven) | None, on track |

---

**Prepared by**: VP Product + Analytics Team
**Next Review**: Q3 2026 (October 1)
**Data Source**: Stripe (revenue), Salesforce (sales), Mixpanel (product), internal support system
