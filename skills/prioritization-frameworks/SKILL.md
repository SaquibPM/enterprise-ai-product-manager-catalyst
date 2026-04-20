---
name: prioritization-frameworks
description: "Master prioritization frameworks for enterprise PM. RICE with formulas, ICE, MoSCoW, Kano, Opportunity Scoring — each with decision trees for when to use. Score features with real enterprise numbers. Stop prioritizing by HiPPO or the loudest customer."
pushy: "You have 50 feature requests and 10 engineers. Gut feel prioritization costs $50k/month in wasted engineering capacity. Real frameworks + metrics = shipping things that move the business needle."
purpose: "Prioritize features that maximize customer value, revenue impact, and product strategy alignment — not just what the CEO asked for."
---

# Prioritization Frameworks for Enterprise Product Management

## Purpose
Prioritization is the hardest part of product management. You have 50 feature requests, 10 engineers, and 3 months of runway. Pick wrong, and you've wasted a quarter on features that don't move revenue or churn. Pick right, and you're the hero.

Enterprise prioritization is harder because it's not just about user happiness. It's about:
- Contract commitments: "We promised this in the contract; when do we ship it?"
- Regulatory deadlines: "GDPR compliance is due in 60 days; everything else waits"
- Customer commitments: "General Dynamics will renew at $5M if we ship X; will churn if we don't"
- Platform dependencies: "Feature Y depends on Platform Refactor; can't start Y until refactor is done"
- Technical debt: "Our API is crashing under load; we need to refactor before we add more features"

This skill teaches you frameworks to score features quantitatively, make defensible decisions, and explain your prioritization to execs, customers, and engineers.

## Key Concepts

### 1. Prioritization Framework Decision Tree

```
START: "I need to prioritize features"

Question 1: Do we have clear, measurable business metrics?
├─ Yes (revenue, retention, churn): Use RICE
├─ Maybe (user satisfaction, effort uncertain): Use ICE
└─ No (exploratory, new market): Use MoSCoW

Question 2: Are there regulatory or contractual deadlines?
├─ Yes: Separate "Committed" queue; prioritize within queue by RICE
└─ No: Use framework above

Question 3: Do we have strong user feedback on desirability?
├─ Yes: Use Kano to bucket features
├─ Maybe: Use Opportunity Scoring
└─ No: Use RICE (assume default desirability)

Question 4: Are we optimizing for growth or retention?
├─ Growth (new customers): Weight acquisition higher in RICE
├─ Retention (keep existing customers): Weight retention higher in RICE
└─ Both: Use equal weighting in RICE

DECISION: Apply framework(s) above
```

### 2. RICE Framework (Recommended for Enterprise)

RICE = Reach × Impact × Confidence / Effort

Each component is quantified:

**Reach**: How many customers/users will this feature reach in the next 3 months?

Examples:
- 100 users from 1 customer (contract commitment) = 100
- 5000 users across 10 customers = 5000
- 50,000 users across all customers = 50,000

**Impact**: How much does this feature improve the user's business?

Scale: 3 = Massive (transformational), 2 = High, 1 = Medium, 0.5 = Low, 0.25 = Minimal

Examples:
- "Reduces contract review time from 60 days to 30 days" = 3 (Massive; saves 6+ weeks)
- "Adds data export in CSV format" = 0.5 (Low; nice-to-have)
- "Improves dashboard load time from 5s to 2s" = 1 (Medium; noticeable but not transformational)
- "Adds dark mode" = 0.25 (Minimal; cosmetic)

**Confidence**: How confident are we this feature will deliver the impact we expect?

Scale: 100% = certain, 80% = confident, 50% = medium confidence, 25% = uncertain

Examples:
- "10 customers asked for this; we demoed it to all 10; 9 said yes" = 100%
- "Market research suggests demand; haven't validated with customers yet" = 50%
- "CEO thinks this is a good idea" = 25%

**Effort**: Engineering estimate in person-weeks (or months)

Examples:
- 2 weeks (small feature) = 2
- 8 weeks (medium feature) = 8
- 20 weeks (major platform change) = 20

**RICE Score Formula**:
```
RICE Score = (Reach × Impact × Confidence) / Effort

Example 1: Contract commitment to General Dynamics
- Reach: 500 users at General Dynamics
- Impact: 3 (transformational; solves pain that's driving churn)
- Confidence: 100% (signed contract; explicitly required)
- Effort: 12 weeks

RICE = (500 × 3 × 100) / 12 = 150,000 / 12 = 12,500

Example 2: CSV export feature
- Reach: 1,000 users across 20 customers (internal requests, not huge demand)
- Impact: 0.5 (nice-to-have; not transformational)
- Confidence: 75% (some customers want it; not everyone)
- Effort: 2 weeks

RICE = (1,000 × 0.5 × 75) / 2 = 375 / 2 = 187.5
```

### 3. ICE Framework (Lighter Than RICE)

ICE = Impact × Confidence / Effort

Use when you don't have clear reach data or when all features impact the same customer base.

**Impact**: 10-point scale (1-10)
- 10 = Massive, transformational
- 7 = High, significant
- 4 = Medium, noticeable
- 1 = Low, minimal

**Confidence**: Percentage (0-100%)
- 100% = Validated with customers
- 75% = Strong signal from data
- 50% = Moderate signal
- 25% = Weak signal

**Effort**: Relative scale (1-10)
- 10 = Massive, 3+ months of engineering
- 7 = Large, 1-2 months
- 4 = Medium, 2-4 weeks
- 1 = Tiny, <1 week

**ICE Score Formula**:
```
ICE Score = (Impact × Confidence) / Effort

Example:
- Impact: 8 (high, solves a real pain)
- Confidence: 75% (customer feedback is strong but not universal)
- Effort: 5 (medium)

ICE = (8 × 75) / 5 = 600 / 5 = 120
```

### 4. MoSCoW Framework (Useful for Roadmap Planning)

MoSCoW categorizes features by business need, not score.

- **Must Have**: Non-negotiable, blocks release, regulatory/contractual requirement
- **Should Have**: High priority, strong customer demand, aligns with strategy
- **Could Have**: Nice-to-have, would be good but isn't critical
- **Won't Have**: Explicitly out of scope; document why (cost, effort, low priority)

Example for Q1 2026 Enterprise AP Automation release:

```
MUST HAVE:
- SOC 2 compliance checklist (security review blocker)
- Invoice matching (core feature, >50 customers request)
- Audit trail logging (HIPAA requirement for healthcare customers)

SHOULD HAVE:
- Duplicate invoice detection (reduces manual work; 20 customers request)
- Supplier email notifications (improves workflow; good-to-have)
- Dashboard drill-down (improves visibility; 5 customers request)

COULD HAVE:
- Dark mode (cosmetic; low effort)
- AI-powered invoice categorization (nice feature; 3 months effort)
- Mobile app (out of scope for this release)

WON'T HAVE (and why):
- Support for Chinese invoices (market not ready; only 2 customers in Asia)
- Integration with Dynamics 365 (Tier 2 integration priority; shift to Q3)
- Custom workflow engine (massive effort; shifts product too much; declined)
```

### 5. Kano Model (For Understanding Feature Desirability)

The Kano Model sorts features by how they impact customer satisfaction.

**Categories**:

1. **Basic Features** (Must-Have)
   - Customers expect these; if missing, satisfaction drops sharply
   - Example: "Invoices can be uploaded"
   - Satisfaction if present: 0 (expected)
   - Satisfaction if absent: -10 (very unsatisfied)

2. **Performance Features** (More-is-Better)
   - More of this = more satisfaction (linear relationship)
   - Example: "Invoices process faster"
   - Satisfaction if present (P95 latency 1sec): +5
   - Satisfaction if present (P95 latency 10sec): +1

3. **Delighter Features** (Wow Factor)
   - Customers don't expect these; if present, satisfaction surges
   - Example: "AI auto-extracts invoice data and categorizes it"
   - Satisfaction if present: +8
   - Satisfaction if absent: 0 (not expected)

**How to Use Kano**:

1. Interview 10-15 customers with two questions per feature:
   - "How satisfied would you be if this feature existed?"
   - "How satisfied would you be if this feature didn't exist?"

2. Score responses: Very Satisfied (+2), Satisfied (+1), Neutral (0), Dissatisfied (-1), Very Dissatisfied (-2)

3. Plot results:
   - Features scoring high on both axes = Basic Features (must-have for parity)
   - Features scoring high on presence, neutral on absence = Performance Features (focus on these)
   - Features scoring low on presence, neutral on absence = Delighters (differentiation)

**Enterprise Example**:

```
Feature: "Audit trail showing who accessed each invoice and when"

Interview 10 customers:
- Q1: If audit trail existed, satisfaction? 
  - 8 customers: Very Satisfied (+2)
  - 2 customers: Neutral (0)
  - Average: +1.6

- Q2: If audit trail didn't exist, satisfaction?
  - 1 customer: Dissatisfied (-1)  /* Healthcare customer; HIPAA requires this */
  - 9 customers: Neutral (0)
  - Average: -0.1

Interpretation: 
High score on presence (+1.6), low/neutral on absence (-0.1) = BASIC FEATURE
Conclusion: Audit trail is table-stakes. Customers expect it for compliance/audit purposes.
If it's missing, healthcare customers churn. Must ship in v1.
```

### 6. Opportunity Scoring (For Customer Research)

Use when you have qualitative feedback and want to quantify it.

**Formula**: 
Importance Score = (Importance × Satisfaction Gap) / 100

Where:
- **Importance**: Scale 1-5, how important is this feature to customers?
- **Satisfaction Gap**: Scale 0-100, how unsatisfied are customers with current solution?

Example:
```
Feature: "Real-time dashboard updates"

Research: Surveyed 20 procurement directors
- Importance rating: 4.2/5 (important, but not critical)
- Current solution satisfaction: 2/10 (very dissatisfied; dashboard refreshes hourly, not real-time)
- Satisfaction Gap: 10 - 2 = 8

Opportunity Score = (4.2 × 8) / 10 = 3.36

Interpretation: High importance + large satisfaction gap = high opportunity
Customers really want this, and current solution is terrible.
This is a high-priority feature.
```

---

## Application: Step-by-Step Prioritization

### Scenario: Enterprise Procurement Platform, Q2 2026 Planning
You have 5 feature requests, 10 engineers, and 13 weeks until release.

**Feature List**:
1. **AI Risk Scoring**: Auto-analyze contracts for risk terms (requested by 10 customers; potential upsell)
2. **Supplier Consolidation**: Merge duplicate supplier records (internal ops team struggling; 2 weeks effort)
3. **Real-Time Dashboard**: Replace hourly refresh with live updates (UI performance issue; 6 weeks effort)
4. **API Versioning Overhaul**: Support multiple API versions simultaneously (engineering debt; no direct revenue)
5. **Supplier Email Notifications**: Email when supplier contract is approved (nice feature; 1 week effort)

### Step 1: Gather Data

**AI Risk Scoring**:
- Customer interviews: 10 of top 15 customers requested this feature
- Revenue impact: Identified 6 customers who will renew/upsell if this ships
  - General Dynamics: Potential $500k deal (contract includes this feature)
  - Boeing: Potential $300k expansion (has requested repeatedly)
  - Lockheed: Considering competing platform if this ships (churn risk)
- Effort estimate: Engineering says 10 weeks (MVP, not fully optimized)
- Confidence: 85% (customer feedback strong; AI model is 95% accurate in pilots)

**Supplier Consolidation**:
- Requested by: Internal ops team and 5 small customers
- Problem it solves: Ops team spends 3 hours/week manually merging duplicates; accounts for 25% of ops time
- Revenue impact: Indirect (saves ops cost, not new revenue; enables customer growth)
- Effort: 2 weeks
- Confidence: 100% (clear problem, clear solution)

**Real-Time Dashboard**:
- Requested by: 7 customers (speed complaints in NPS feedback)
- Revenue impact: Not directly tied to sales; retention issue (customers find slow performance frustrating)
- Effort: 6 weeks
- Confidence: 60% (customers want it, but not clear if speed improvement alone drives retention or if other factors matter more)

**API Versioning Overhaul**:
- Requested by: Engineering team (technical debt; wants to avoid breaking customers when API changes)
- Revenue impact: None directly; enables future features by avoiding customer breaking changes
- Effort: 8 weeks
- Confidence: 75% (obvious technical need; improves reliability)

**Supplier Email Notifications**:
- Requested by: 3 customers (workflow improvement)
- Revenue impact: None (feature parity, not differentiator)
- Effort: 1 week
- Confidence: 100% (straightforward feature)

### Step 2: Apply RICE Framework

**AI Risk Scoring**:
- Reach: 10 customers requesting + 6 customers identified for upsell = 16 customers, ~4,000 users
- Impact: 3 (transformational; solves major pain point; drives upsell)
- Confidence: 85%
- Effort: 10 weeks

RICE = (4,000 × 3 × 85) / 10 = 10,200 / 10 = **1,020**

**Supplier Consolidation**:
- Reach: Internal team + 5 customers, ~50 users impacted (small blast radius)
- Impact: 2 (high; saves ops team 12 hours/week; reduces manual effort)
- Confidence: 100%
- Effort: 2 weeks

RICE = (50 × 2 × 100) / 2 = 100 / 2 = **50**

**Real-Time Dashboard**:
- Reach: 7 customers requesting + potentially all customers (everyone uses dashboard), ~30,000 users
- Impact: 2 (high; improves user experience; affects retention)
- Confidence: 60% (not sure if speed alone drives retention)
- Effort: 6 weeks

RICE = (30,000 × 2 × 60) / 6 = 36,000 / 6 = **6,000**

**API Versioning Overhaul**:
- Reach: Engineering team (internal) + enables future features, ~5 future feature impact
- Impact: 1 (medium; technical enabler; doesn't directly improve user experience)
- Confidence: 75%
- Effort: 8 weeks

RICE = (5 × 1 × 75) / 8 = 3.75 / 8 = **0.47**

(Very low RICE score because reach is minimal and impact is indirect)

**Supplier Email Notifications**:
- Reach: 3 customers requesting, ~300 users
- Impact: 1 (medium; workflow improvement, not transformational)
- Confidence: 100%
- Effort: 1 week

RICE = (300 × 1 × 100) / 1 = 300 / 1 = **300**

### Step 3: Rank Features by RICE Score

```
Rank | Feature | RICE Score | Effort (weeks) |
-----|---------|-----------|---------|
1    | AI Risk Scoring | 1,020 | 10 |
2    | Real-Time Dashboard | 6,000 | 6 |
3    | Supplier Email Notifications | 300 | 1 |
4    | Supplier Consolidation | 50 | 2 |
5    | API Versioning Overhaul | 0.47 | 8 |
```

Wait, this looks wrong. RICE Score doesn't increase linearly with effort; let me recalculate Real-Time Dashboard:

RICE = (30,000 × 2 × 60) / 6 = 36,000 / 6 = 6,000

Yes, that's correct. Real-Time Dashboard has a higher RICE score (6,000 vs 1,020) because it affects 30,000 users vs 4,000 users.

But effort is also higher (6 weeks vs 10 weeks). Total effort for top 2 features = 16 weeks, exceeds our 13-week capacity.

### Step 4: Resolve Capacity Constraints

We have 13 weeks, top 2 features total 16 weeks. We need to drop one or defer one.

**Option A: Ship AI Risk Scoring (highest ROI per effort)**
- Effort: 10 weeks
- Remaining capacity: 3 weeks
- Add Supplier Email Notifications (1 week) = 11 weeks total
- Defer: Real-Time Dashboard, API Versioning, Supplier Consolidation

**Option B: Ship Real-Time Dashboard (biggest reach)**
- Effort: 6 weeks
- Remaining capacity: 7 weeks
- Add Supplier Email Notifications (1 week) + Supplier Consolidation (2 weeks) = 9 weeks total
- Defer: AI Risk Scoring, API Versioning

**Option C: Ship Both, Accept Risk**
- Effort: 16 weeks
- Capacity: 13 weeks
- Aggressive planning; high risk of slipping
- Result: Ship AI Risk Scoring (10 weeks) + Real-Time Dashboard (6 weeks) = 100% capacity
- Assume no surprises, no bugs, no refactoring
- Reality: 30% chance of 2-4 week slip

**Decision Criteria**:

Which decision optimizes for business impact?

**Option A Rationale**:
- AI Risk Scoring has highest RICE score per unit effort (RICE / Effort = 1,020 / 10 = 102)
- Directly drives $500k+ in new revenue (General Dynamics contract)
- Reduces churn risk (Lockheed competition)
- Email notifications is low-effort, low-risk add-on (1 week)
- Defer Real-Time Dashboard to Q3 (still important, but not critical to Q2 revenue)

**Option B Rationale**:
- Real-Time Dashboard has highest absolute RICE score (6,000) due to reach
- Impacts all 30,000 users (broad impact)
- Improves NPS and retention
- Include Supplier Consolidation (ops team unblock, saves 12 hours/week)
- Defer AI Risk Scoring to Q3 (delay revenue, but less risk)

**Recommendation: Option A (AI Risk Scoring Priority)**

Why: Contract commitment (General Dynamics deal is $500k) + churn risk (Lockheed) + highest ROI per effort + lower execution risk (focused scope).

Real-Time Dashboard is important for retention but can ship in Q3 without major revenue impact. AI Risk Scoring, if delayed, delays $500k contract signature.

### Step 5: Document Priority Rationale

```
Q2 2026 Feature Prioritization

Approved for Q2:
1. AI Risk Scoring (RICE: 1,020; Revenue impact: $500k+; Churn risk reduction)
   - Status: Committed; contract attached
   - Effort: 10 weeks
   - Dependency: None
   
2. Supplier Email Notifications (RICE: 300; Nice-to-have workflow improvement)
   - Status: Approved (low effort; fits remaining capacity)
   - Effort: 1 week
   - Dependency: Supplier API (already shipped)

Deferred to Q3:
1. Real-Time Dashboard (RICE: 6,000; Effort: 6 weeks)
   - Reason: Capacity constraint; lower revenue impact than Q2 priorities
   - Will improve retention, but not critical for Q2 metrics
   - Still high priority for Q3
   
2. Supplier Consolidation (RICE: 50; Effort: 2 weeks)
   - Reason: Operational efficiency issue; not customer-facing
   - Defer 1 quarter; ops can optimize workflows manually in interim
   
3. API Versioning Overhaul (RICE: 0.47; Effort: 8 weeks)
   - Reason: Technical debt with low near-term impact
   - Does not directly drive customer value in Q2
   - Defer; re-evaluate for Q4 planning alongside other tech debt

Total Approved Effort: 11 weeks (out of 13 capacity)
Buffer: 2 weeks (for unknowns, bugs, testing)
```

---

## Worked Example: Prioritizing 5 Features with RICE

### Scenario: Enterprise AI Product Manager at Procurement SaaS, Q1 2026

You are building features for the next 12-week quarter. You have:
- 12 engineers (full capacity)
- 15 feature requests
- Goal: Increase ARR by 10% (from $10M to $11M) and reduce churn from 12% to 10%

### Feature Requests (Simplified)

1. **Multi-currency Support**: Contracts in EUR, GBP, JPY (requested by 8 customers; 5 weeks effort)
2. **AI-Powered Audit Trail**: Auto-generate audit logs for compliance (requested by 2 large customers; 4 weeks effort)
3. **Dashboard Customization**: Let admins build custom dashboards (requested by 3 customers; 6 weeks effort)
4. **Bulk Upload with Validation**: Upload 1000 contracts, validate before import (requested by 4 customers; 3 weeks effort)
5. **Supplier Scorecards**: Auto-generate supplier performance reports (requested by 5 customers; 8 weeks effort)

### RICE Analysis

**Feature 1: Multi-Currency Support**

Market research: 8 customers requested; 3 are in Europe and EMEA expansion is strategy for 2026
- Reach: 8 customers × 500 users per customer = 4,000 users
- Impact: 2 (high; enables EMEA expansion; unlocks new market)
- Confidence: 75% (customer requests are consistent; not all customers will use immediately)
- Effort: 5 weeks

RICE = (4,000 × 2 × 75) / 5 = 6,000 / 5 = **1,200**

**Feature 2: AI-Powered Audit Trail**

Scope: Auto-generate GDPR/SOC 2 compliance audit trails from contract events

Market: 2 very large customers (Global Fortune 500) requesting; security team identified as must-have
- Reach: 2 customers × 2,000 users per customer (larger orgs) = 4,000 users
- Impact: 3 (massive; solves compliance blocker; required for enterprise deals; 3 customers in pipeline waiting for this)
- Confidence: 100% (explicit contract requirements; non-negotiable)
- Effort: 4 weeks

RICE = (4,000 × 3 × 100) / 4 = 12,000 / 4 = **3,000**

**Feature 3: Dashboard Customization**

Scope: Admins can build custom dashboards; drag-drop widgets

Market: 3 customers requested; common feature in competitor products
- Reach: 3 customers × 300 admin users per customer = 900 users (admins only, not end-users)
- Impact: 1 (medium; nice-to-have; doesn't solve core pain; parity feature)
- Confidence: 50% (some demand, but not universally requested; lower confidence in actual usage)
- Effort: 6 weeks

RICE = (900 × 1 × 50) / 6 = 450 / 6 = **75**

**Feature 4: Bulk Upload with Validation**

Scope: Upload 1000 contracts at once; server validates all before importing

Market: 4 customers with large contract portfolios requested; ops workflow improvement
- Reach: 4 customers × 50 ops users per customer = 200 users
- Impact: 2 (high; solves workflow pain for ops teams; saves hours per week)
- Confidence: 100% (clear problem; straightforward solution; all customers validated)
- Effort: 3 weeks

RICE = (200 × 2 × 100) / 3 = 400 / 3 = **133**

**Feature 5: Supplier Scorecards**

Scope: Auto-generate supplier performance scorecards with metrics

Market: 5 customers requesting; would improve supplier management; potential upsell feature
- Reach: 5 customers × 400 procurement users per customer = 2,000 users
- Impact: 2 (high; solves supplier visibility problem; potential upsell at +$50k/customer)
- Confidence: 70% (customer feedback positive; actual upsell rates uncertain)
- Effort: 8 weeks

RICE = (2,000 × 2 × 70) / 8 = 2,800 / 8 = **350**

### Prioritization Table

```
Rank | Feature | RICE | Effort | ROI (RICE/Effort) | Revenue Impact |
-----|---------|------|--------|-------------------|-----------------|
1    | AI Audit Trail | 3,000 | 4 | 750 | Retention, contract requirement |
2    | Multi-Currency | 1,200 | 5 | 240 | EMEA expansion, new customers |
3    | Supplier Scorecards | 350 | 8 | 43.75 | Potential upsell: +$250k/year |
4    | Bulk Upload | 133 | 3 | 44.33 | Ops efficiency, retention |
5    | Dashboard Customization | 75 | 6 | 12.5 | Parity, low priority |
```

### Capacity Planning (12-week quarter)

```
Total engineering capacity: 12 engineers × 12 weeks = 144 engineer-weeks
Buffer for unknowns: 20% = 29 engineer-weeks
Available for features: 144 - 29 = 115 engineer-weeks

Features ranked by RICE:
1. AI Audit Trail: 4 weeks ✓ (Total: 4, Remaining: 111)
2. Multi-Currency: 5 weeks ✓ (Total: 9, Remaining: 106)
3. Supplier Scorecards: 8 weeks ✓ (Total: 17, Remaining: 98)
4. Bulk Upload: 3 weeks ✓ (Total: 20, Remaining: 95)
5. Dashboard Customization: 6 weeks ✓ (Total: 26, Remaining: 89)

All features fit within capacity!
```

### Decision: Approve All 5 Features

**Rationale**:
- AI Audit Trail (RICE: 3,000): Contract requirement; non-negotiable; highest ROI
- Multi-Currency (RICE: 1,200): Strategic priority (EMEA expansion); enables $2M+ TAM
- Supplier Scorecards (RICE: 350): Upsell feature; potential +$250k/year revenue
- Bulk Upload (RICE: 133): Ops improvement; retention driver; low effort
- Dashboard Customization (RICE: 75): Lowest priority; but fits in capacity and provides parity

**Sequencing**:
```
Weeks 1-4: AI Audit Trail (blocking; must complete first)
Weeks 1-5: Multi-Currency (parallel track; no dependency on audit trail)
Weeks 6-13: Supplier Scorecards (after Multi-Currency baseline)
Weeks 1-3: Bulk Upload (quick win; can happen in parallel)
Weeks 6-12: Dashboard Customization (lowest priority; starts after high-priority features stable)

Buffer weeks 13-12 (1 week): Testing, bug fixes, unexpected issues
```

**Expected Business Impact**:
- Retention: AI Audit Trail + Bulk Upload → improve retention by 2% (12% → 10%)
- Revenue: Multi-Currency ($500k new EMEA deals) + Scorecards upsell ($250k)
- Total: ARR grows by $750k (from $10M to $10.75M) = 7.5% growth (vs. 10% goal; shortfall due to execution risk)

---

## Common Pitfalls

### Pitfall 1: HiPPO Prioritization (Highest Paid Person's Opinion)
**Anti-Pattern**: "The CEO wants dark mode. Shift all features; dark mode is priority #1."

**Why it fails**: Dark mode has RICE score of 10 (cosmetic). Your actual priority is a contract requirement with RICE of 3,000. You spend 4 weeks on dark mode; the contract requirement slips. Customer doesn't renew. $500k deal lost.

**Right way**: Present RICE scores to the CEO. Show dark mode's actual impact (10) vs. contract requirement (3,000). CEO will usually defer dark mode when they see the math.

If CEO insists: Document the business case for deferral in writing. Then, if dark mode ships and contract fails, you have a record showing you raised the risk.

### Pitfall 2: Prioritizing by Loudest Customer
**Anti-Pattern**: Customer calls, demands feature X, threatens to churn. You drop everything; feature X becomes priority #1.

**Why it fails**: One loud customer is rarely representative of the market. You prioritize based on 1 customer instead of 100. Meanwhile, 100 customers are waiting for features that would help them more.

**Right way**: Use RICE. If the customer is a $500k annual account, factor that into Reach. If only 1 customer requested it, Reach is 1 customer, and RICE score will be lower than a feature 10 customers requested.

If customer threatens churn: Ask them to quantify (RICE). How much will they pay for this feature? How many of their workflows does it unblock? Then add their contribution to the RICE score. If it's still low priority, be honest: "This is lower priority than our contract commitments, but we'll revisit it in Q4 if we have capacity."

### Pitfall 3: Ignoring Effort in Prioritization
**Anti-Pattern**: "Feature X has high RICE score; ship it!" Engineering comes back: "But it's 20 weeks of work and breaks our API."

**Why it fails**: You've committed to something that creates technical debt or takes 2 quarters instead of 1. ROI per effort matters more than absolute RICE score.

Feature A: RICE = 1,000, Effort = 10 weeks, ROI = 100
Feature B: RICE = 500, Effort = 1 week, ROI = 500

Feature B has lower absolute RICE but higher ROI per effort. In constrained capacity, Feature B is the better investment.

**Right way**: Calculate ROI per effort (RICE / Effort). Prioritize by ROI, not absolute RICE. This accounts for both impact and feasibility.

### Pitfall 4: Treating All Frameworks Equally
**Anti-Pattern**: You use RICE for everything, even when RICE doesn't apply.

Example: You're exploring a new market (e.g., new geographies). You don't have customer data yet. RICE will give you false precision.

**Why it fails**: RICE requires real customer feedback and clear reach numbers. If you're exploring, you don't have that. You'll score exploratory features as low-priority, then wonder why they never ship.

**Right way**: Use framework that matches your context.
- RICE: You have clear customer data, reach estimates, and revenue metrics (most common)
- ICE: You don't have reach data but have impact and confidence (lighter-weight)
- MoSCoW: You have hard constraints (contracts, regulatory deadlines) that override scoring
- Kano: You want to understand desirability before investing effort
- Opportunity Scoring: You have qualitative feedback but need to quantify it

### Pitfall 5: Not Weighting for Strategic Alignment
**Anti-Pattern**: RICE scores features objectively. But your strategy is "expand into mid-market." A feature that helps mid-market should be weighted higher.

**Why it fails**: You optimize for short-term revenue but miss strategic bets. You ship features for large enterprise accounts, ignore mid-market needs, and fall behind in your own strategy.

**Right way**: Add a "Strategic Weight" multiplier to RICE.

```
Adjusted RICE = RICE × Strategic Weight

Strategic weights:
- Expand mid-market: Weight mid-market features by 1.5x
- Focus on enterprise: Weight enterprise features by 1.5x
- Build platform: Weight foundational features by 1.2x

Example:
Feature: "Supplier Scorecards" (mid-market request)
RICE: 350
Strategic weight: 1.5x (because mid-market expansion is strategy)
Adjusted RICE: 350 × 1.5 = 525

This bumps mid-market features up in priority, aligning prioritization with strategy.
```

### Pitfall 6: Conflating Effort Estimates with Effort Reality
**Anti-Pattern**: Engineering says "2 weeks." You plan around 2 weeks. It takes 6 weeks. You've under-committed.

**Why it fails**: 30% of projects slip. If you have 13 weeks and estimate 13 weeks of features, 30% chance you miss your release date.

**Right way**: Use confidence-weighted effort.

```
Engineering estimate: 2 weeks
Your confidence: 50% (first time building this; uncertain)
Padded effort: 2 weeks / 50% = 4 weeks

Or: Create an effort buffer.
Planned capacity: 13 weeks
Buffer for unknowns: 20% = 2.6 weeks
Available for features: 10.4 weeks

Now you only commit to 10 weeks of features.
Remaining 3 weeks = buffer for surprises.
```

### Pitfall 7: Not Tracking Prioritization Outcomes
**Anti-Pattern**: You prioritized Feature X over Feature Y based on RICE. 6 months later, did Feature X actually deliver the impact you expected?

**Why it fails**: You never learn if your prioritization framework works. You keep making the same mistakes.

**Right way**: Post-mortem each release.

```
Feature: AI Risk Scoring
RICE Estimate: 1,020
- Reach estimate: 4,000 users
- Impact estimate: 3
- Actual reach: 2,500 users (smaller than expected)
- Actual impact: 2.5 (customers used it less than expected)
- Actual RICE: 625 (lower than estimate)

Learning: Reach estimates were off. Next time, apply 40% discount to reach estimates for new features (customers don't adopt as fast as expected).

Adjust future RICE scores accordingly.
```

---

## References

- **Intercom: RICE Scoring**: https://www.intercom.com/blog/rice-formula-prioritization/
- **Marty Cagan, "Empowered"**: Prioritization frameworks for product leaders
- **Kano Model Research**: Japanese customer satisfaction theory; used in UX research
- **Reforge: Prioritization Course**: ICE and other frameworks
- **Product School: Prioritization Frameworks**: MoSCoW, Kano, Opportunity Scoring

---

## Next Steps

1. **Pick one framework for your next prioritization**: Start with RICE (most common for enterprise)
2. **Gather RICE data**: Interview customers for reach; estimate impact; assign confidence
3. **Calculate scores**: Apply formula; rank by ROI per effort
4. **Pressure-test results**: Do the top-ranked features align with strategy and business goals?
5. **Get buy-in**: Share scores and reasoning with engineering and executive team
6. **Track outcomes**: At end of quarter, measure actual impact vs. estimated RICE
7. **Iterate**: Refine your estimation process based on actual results
